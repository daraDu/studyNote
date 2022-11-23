# node+express-swagger-generator 生成接口文档

> 使用node和express-swagger-generator 自动生成接口文档

https://www.npmjs.com/package/express-swagger-generator

> 示例地址：https://github.com/daraDu/nodeVue3App/tree/main/server

## 安装插件

```
npm i express-swagger-generator --save-dev
```

## 使用

### 引入

```js
// index.js
const express = require('express')

const app = express()
//生成这个密钥生成token，一般这个存储在环境变量中，这里简便处理
app.set('secret', 'ssdasdad')
app.set('apiCommon', '/manHours')
// 解决跨域
app.use(require('cors')())
// 通过 express.json() 这个中间件，解析表单中的 JSON 格式的数据
app.use(express.json())

// 使用swagger API 文档
const expressSwagger = require('express-swagger-generator')(app)
let options = {
    swaggerDefinition: {
        info: {
            description: '工时系统h5+管理后台共用接口api',
            title: 'api',
            version: '1.0.0'
        },
        host: 'localhost:9000',
        basePath: '/',
        produces: ['application/json', 'application/xml'],
        schemes: ['http', 'https'],
        securityDefinitions: {
            JWT: {
                type: 'apiKey',
                in: 'header',
                name: 'Authorization',
                description: ''
            }
        }
    },
    route: {
        url: '/swagger',
        docs: '/swagger.json' //swagger文件 api
    },
    basedir: __dirname, //app absolute path
    files: ['./models/*.js', './routes/**/*.js'] //Path to the API handle folder 模块文件路径以及路由文件路径
}

expressSwagger(options)

require('./routes/admin/index.js')(app)

app.listen(9001, () => {
    console.log('http://localhost:9001/');
})
```

### 路由中使用

```js
module.exports = (app) => {
  const mongoose = require('mongoose');
  const express = require('express');
  const router = express.Router();
  const UserModel = require('../../models/User.js');
  const UserSchemaMod = require('../../models/User.js');
  const UserProjectModel = require('../../models/UserProject.js');
  const authMiddleware = require('../../middleware/auth');

  /**
   * 查询用户数据列表
   * @route GET /manHours/user/lists
   * @group user - 用户模块
   * @param {number} page.query.required - 当前页码
   * @param {string} pageSize.query.required - 每页数据条数
   * @param {number} isAdmin.query - 1-管理员 2-用户
   * @param {string} name.query - 成员姓名/手机号
   * @returns {UserSchemaModel.model} 200 - An array of user info // UserSchemaModel对应下面模型中的UserSchemaModel
   * @returns {Error}  default - Unexpected error
   * @security JWT
   */
  router.get('/user/lists', async (req, res) => {
    let { isAdmin, page, pageSize } = req.query;

    // 模糊查询
    const name = new RegExp(req.query.name, 'i');
    // 多条件查询
    let query = {};
    if (name) {
      query.$or = [{ name: name }, { phone: name }];
    }
    if (isAdmin) {
      query.isAdmin = Number(isAdmin);
    }
    let totalCount = await UserModel.countDocuments(query);
    // 分页
    page = parseInt(page) || 1;
    UserModel.aggregate(
      [
        { $match: query },
        { $skip: (page - 1) * pageSize },
        { $limit: Number(pageSize) },
        {
          $lookup: {
            // 关联查询
            from: 'userProject', // 关联数据库中 userProject 文档
            foreignField: 'userId', // userProject 文档中的 _id 字段
            localField: '_id', // user 文档中的 _id 字段
            as: 'projectLists', // 将查询的数据放至 items 字段中
          },
        },
        {
          $group: {
            _id: '$_id',
            detail: { $first: '$$ROOT' },
            projectNum: {
              $sum: {
                $size: '$projectLists',
              },
            },
          },
        },
        {
          $replaceRoot: {
            newRoot: {
              $mergeObjects: [{ projectNum: '$projectNum' }, '$detail'],
            },
          },
        },
        {
          $project: {
            projectLists: 0,
            password: 0,
          },
        },
        { $sort: { _id: -1 } }, // 插入时间来排序
      ],
      (err, result) => {
        if (err) {
          res.send({ code: 500, message: err.message, data: null });
          return;
        }
        res.send({
          code: 200,
          message: '查询成功',
          data: result,
          total: totalCount,
        });
      }
    );
  });

  /**
   * 用户工时详情
   *   @param {string} id 用户id
   * */
  app.use(app.get('apiCommon'), authMiddleware(), router);
};
```

### 模型中定义

```js
const mongoose = require('mongoose')
/**
 * @typedef UserSchemaModel  // 对应上面路由中的UserSchemaModel
 * @property {string} username - 用户名
 * @property {string} name  - 姓名
 */
const UserSchema = new mongoose.Schema({
	username: {
		type: String,
		index: true
	},
	name: {
		type: String,
		index: true
	},
	phone: {
		type: String,
		index: true
	},
	// positionId: {type: mongoose.Schema.Types.ObjectId}, 
	status: { // 1-启用 2-禁用 默认启用
		type: Number,
		index: true
	},
	isAdmin: {
		type: Number,
		index: true
	},
	password: {
		type: String,
		//查询这个模型的时候不查询password这个字段
		select: false,
		//密码需要加密
		set(val) {
			var bcrypt = require('bcryptjs');
			//加密强度为10
			var salt = bcrypt.genSaltSync(10);
			//使用bcrypt库的散列（同步）方法对值进行加密后再存入数据库
			return bcrypt.hashSync(val, salt)
		}
	},
}, {
	// 自动添加 createdAt & updatedAt 字段
	timestamps: true
})

module.exports = mongoose.model('user', UserSchema, 'user')
```

### 图示

![图示](E:\test\笔记\image\node\nodeSwagger.png)

### 如果model模型里的字段不满足接口返回的字段

> 另外创建一个typedef

```js
/**
   * 查询用户数据列表
   * @route GET /manHours/user/lists
   * @group user - 用户模块
   * @param {number} page.query.required - 当前页码
   * @param {string} pageSize.query.required - 每页数据条数
   * @param {number} isAdmin.query - 1-管理员 2-用户
   * @param {string} name.query - 成员姓名/手机号
   * @returns {Array.<userListsRet.model>} 200 - An array of user info
   * @returns {Error}  default - Unexpected error
   * @security JWT
   */
  // 返回字段
  /**
   * @typedef userListsRet
    * @property {string} username - 用户名
    * @property {string} name  - 姓名
    * @property {string} phone  - 手机号码
    * @property {integer} status  - 1-启用 2-禁用 默认启用
    * @property {integer} isAdmin  - 1-管理员 2-用户
    * @property {string} positionId  - 职位id
    * @property {string} createdAt  - 创建时间
    * @property {integer} projectNum  - 参与项目数
   */
  router.get('/user/lists', async (req, res) => {
    let { isAdmin, page, pageSize } = req.query;

    // 模糊查询
    const name = new RegExp(req.query.name, 'i');
    // 多条件查询
    let query = {};
    if (name) {
      query.$or = [{ name: name }, { phone: name }];
    }
    if (isAdmin) {
      query.isAdmin = Number(isAdmin);
    }
    let totalCount = await UserModel.countDocuments(query);
    // 分页
    page = parseInt(page) || 1;
    UserModel.aggregate(
      [
        { $match: query },
        { $skip: (page - 1) * pageSize },
        { $limit: Number(pageSize) },
        { $sort: { _id: -1 } }, // 插入时间来排序
      ],
      (err, result) => {
        if (err) {
          res.send({ code: 500, message: err.message, data: null });
          return;
        }
        res.send({
          code: 200,
          message: '查询成功',
          data: result,
          total: totalCount,
        });
      }
    );
  });
```

### post接口 接收query字段

```js
/**
   * 项目阶段列表
   * @route post /manHours/login
   * @group user - 用户模块
   * @param {loginBody.model} loginRet.body.required - the new point
   * @returns {loginRet.model} 200 - An array of user info
   * @returns {Error}  default - Unexpected error
   */
  /**
   * @typedef loginBody
    * @property {string} username  - 用户名
    * @property {string} password  - 密码
    * @property {number} channel  - 来源  1 web端登录 2 h5端登录
   */
  // 返回字段
  /**
   * @typedef loginRet
    * @property {string} token  - token
    * @property {string} id  - 用户id
    * @property {string} name  - 姓名
    * @property {string} username  - 用户名称
   */
```

### 访问：http://localhost:9000/swagger/#/?docExpansion=none

docExpansion=none 默认不展开（为了隐藏models）

