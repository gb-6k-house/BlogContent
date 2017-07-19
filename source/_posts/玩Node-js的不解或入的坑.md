---
title: 玩Node.js的不解或入的坑
date: 2017-07-18 17:35:49
tags: [技术之路,后台, Nodejs, Sequelize, Promise, RPC, hprose]
---
#### Sequelize, Promise, hprose结合入坑
请看看代码
```
 /**
     * 获取机构的信息
     * 
     * @static
     * @param {*} token 
     * @returns {Promise<any>} 
     * @memberof TUserRpcImpl
     */
    public static orgnazationInfo(token: any): Promise<any> {
        logger.info("进入。。orgnazationInfo");
        return new Promise(function (resolve, reject) {
            TUserRpcImpl.userInfo(token).then((result) => {
                //token有可能无效， vertifyToken返回1,或 2
                if (typeof result === 'object') {
                    userTable.findOne({ where: { id: result.id }, include: [{ model: organizationTable }] }).then((result) => {
                        logger.info('机构信息', result)
                        if (result.tOrganization == null) {
                            resolve(Code.orgnizationNotExist)
                        } else {
                            let o: any = {}
                            o.userId = result.id
                            o.phone = result.phone
                            o.userName = result.userName
                            o.organization = result.tOrganization
                            resolve(o)
                        }
                    }).catch(function (err) {
                        //token失效
                        logger.info('orgnazationInfo 报错', err)

                        reject(err);
                    })

                } else {
                    resolve(result)
                }
            }).catch(function (err) {
                //token失效
                logger.info('orgnazationInfo 报错', err)

                reject(err);
            })
        });
    }
```
说明：
1、orgnazationInfo 方法将作为RPC接口方法通过hprose发布。
2、userTable，organizationTable通过Sequelize建立了一对一的关系。
3、现在通过userTable管理查询organizationTable数据，然后Promise形式返回


问题：
如果通过直接把Sequelize.findOne获取的organizationTable对象直接复制给返回对象即
```
o.organization = result.tOrganization
```
在hprose客户端调用方法orgnazationInfo时会抛出异常 “map.hasOwnProperty is not a function” 

但是如果把直接赋值修改为：
```

o.organization = {}
o.organization.name = result.tOrganization.name
...
```
则不会抛出异常！！！！
