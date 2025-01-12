---
title: 迁移
intro: 'Migration API 可让您将组织的仓库和用户从 {% data variables.product.prodname_dotcom_the_website %} 迁移到 {% data variables.product.prodname_ghe_server %}。'
redirect_from:
  - /v3/migrations
  - /v3/migration
  - /v3/migration/migrations
versions:
  fpt: '*'
  ghec: '*'
  ghes: '>3.3'
  ghae: issue-6184
topics:
  - API
miniTocMaxHeadingLevel: 3
---

{% for operation in currentRestOperations %}
  {% unless operation.subcategory %}{% include rest_operation %}{% endunless %}
{% endfor %}

## 组织

迁移 API 仅适用于经过身份验证的组织所有者。 更多信息请参阅“[组织中的角色](/organizations/managing-peoples-access-to-your-organization-with-roles/roles-in-an-organization#permission-levels-for-an-organization)”和“[其他身份验证方法](/rest/overview/other-authentication-methods)”。

{% data variables.migrations.organization_migrations_intro %}

{% for operation in currentRestOperations %}
  {% if operation.subcategory == 'orgs' %}{% include rest_operation %}{% endif %}
{% endfor %}

{% ifversion fpt or ghec %}
## 源导入

{% data variables.migrations.source_imports_intro %}

如果导入中存在大型文件，则典型的源导入将开始导入，然后（可选）更新作者和/或更新使用 Git LFS 的首选项。 您也可以创建侦听 [`RepositoryImportEvent`](/developers/webhooks-and-events/webhook-events-and-payloads#repository_import) 的 web 挂钩来了解导入的状态。

在此图中可以看到更详细的示例：

```
+---------+                     +--------+                              +---------------------+
| Tooling |                     | GitHub |                              | Original Repository |
+---------+                     +--------+                              +---------------------+
     |                              |                                              |
     |  Start import                |                                              |
     |----------------------------->|                                              |
     |                              |                                              |
     |                              |  Download source data                        |
     |                              |--------------------------------------------->|
     |                              |                        Begin streaming data  |
     |                              |<---------------------------------------------|
     |                              |                                              |
     |  Get import progress         |                                              |
     |----------------------------->|                                              |
     |       "status": "importing"  |                                              |
     |<-----------------------------|                                              |
     |                              |                                              |
     |  Get commit authors          |                                              |
     |----------------------------->|                                              |
     |                              |                                              |
     |  Map a commit author         |                                              |
     |----------------------------->|                                              |
     |                              |                                              |
     |                              |                                              |
     |                              |                       Finish streaming data  |
     |                              |<---------------------------------------------|
     |                              |                                              |
     |                              |  Rewrite commits with mapped authors         |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |                              |  Update repository on GitHub                 |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |  Map a commit author         |                                              |
     |----------------------------->|                                              |
     |                              |  Rewrite commits with mapped authors         |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |                              |  Update repository on GitHub                 |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |  Get large files             |                                              |
     |----------------------------->|                                              |
     |                              |                                              |
     |  opt_in to Git LFS           |                                              |
     |----------------------------->|                                              |
     |                              |  Rewrite commits for large files             |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |                              |  Update repository on GitHub                 |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |  Get import progress         |                                              |
     |----------------------------->|                                              |
     |        "status": "complete"  |                                              |
     |<-----------------------------|                                              |
     |                              |                                              |
     |                              |                                              |
```

{% for operation in currentRestOperations %}
  {% if operation.subcategory == 'source-imports' %}{% include rest_operation %}{% endif %}
{% endfor %}
{% endif %}
## 用户

用户迁移 API 仅适用于经过身份验证的帐户所有者。 更多信息请参阅“[其他身份验证方法](/rest/overview/other-authentication-methods)”。

{% data variables.migrations.user_migrations_intro %} 有关可下载的迁移数据列表，请参阅“[下载用户迁移存档](#download-a-user-migration-archive)”。

要下载存档，您需要先开始用户迁移。 在迁移状态为 `exported` 后，便可下载迁移。

创建迁移存档后，该存档可供下载七天。 但是，如果您喜欢，您可以更早删除用户迁移存档。 当迁移为 `exported` 时，您可以解锁仓库以便再次开始使用，如果你不再需要源数据，则可删除您的仓库。

{% for operation in currentRestOperations %}
  {% if operation.subcategory == 'users' %}{% include rest_operation %}{% endif %}
{% endfor %}
