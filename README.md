# HeyForm 懒猫微服包

表单搭建工具：拖拽式制作问卷、测验、投票与报名表。

- 上游：<https://github.com/heyform/heyform>
- 镜像：`heyform/community-edition`
- 许可：AGPL-3.0（全文见 `content/licenses/LICENSE`，仓库无子目录覆盖条款）

## 组成

| 服务 | 镜像 | 作用 |
| --- | --- | --- |
| `heyform` | `heyform/community-edition` | 应用本体，监听 9157 |
| `mongo` | `mongo:8.0` | 表单与答卷存储 |
| `redis` | `redis:8-alpine` | Bull 后台任务队列 |

## 关键配置说明

- `APP_HOMEPAGE_URL` 必须是用户实际访问的地址，官方文档明确提到设错会导致登录问题。
- `TRUST_PROXY` 必须开启：入口在容器前面转发，不信任代理头会导致来源地址判断错误。
- `SESSION_KEY` / `FORM_ENCRYPTION_KEY` 必须覆盖：镜像默认值是字面量 `key1` / `key2`，
  留着会让会话与加密的表单数据可被伪造。
- `VERIFY_USER_EMAIL` 必须为 `false`：上游默认要求邮箱验证码才能完成注册，
  而本包未配置 SMTP，官方注释也写明"没有 SMTP 时没人能完成注册"。
- 应用自带账号体系，因此 `public_path` 开放根路径，由应用自己鉴权。

## 未验证部分

未在真机上实测：注册登录、创建表单、提交答卷、逻辑跳转、重启后数据留存。
