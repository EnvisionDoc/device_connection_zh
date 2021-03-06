# 批量管理告警配置

为了提升配置效率，告警服务支持批量操作。你可以将多个告警的配置导出成Excel文件，在文件中完成编辑后再导入，以完成批量创建多个配置或对多个配置的更新。

当前批量操作在以下告警配置可获取：

- **告警级别**：批量导出全部信息，包括组织、级别编号、级别描述、标签。
- **告警类型**：批量导出全部信息，包括组织、类型编号、类型描述、标签。
- **告警内容**：批量导出全部信息，包括组织、内容编号、内容描述、标签、模型、类型。
- **告警规则**：批量导出全部信息，包括组织、规则编号、规则描述、模型、测点、触发条件、告警内容编号、告警级别编号、标签、作用域、是否生效、是否是根源告警。

## 操作步骤

该步骤以**告警级别**为例描述如何进行批量编辑或创建。其他告警配置的批量操作与**告警级别**相似，可参考该步骤。

1. 在EnOS Console的导航菜单中，点击**告警管理 > 告警级别**。

2. 选择一个或多个记录，点击**批量导出**。编辑导出的Excel文件。

   - 如需编辑多个告警级别记录，勾选所需编辑的所有记录，点击**批量导出**。
   - 如需批量创建新的告警级别记录，勾选一条已有记录，点击**批量导出**。导出的Excel表格即可当做模板。按照模板中的格式，新增表格行，每一行代表一条记录。
   - 你也可以在导出的表格中同时对已有记录进行编辑，及新增记录。

3. 完成并保存对Excel的编辑后，点击**批量导入**。
   选择导入Excel文件后，EnOS会对文件中数据的有效性进行校验并提示导入成功与失败的条数。对于未成功导入的记录，点击**导出无效记录**查看失败原因，根据提示解决问题后再次尝试。

    ![](media/alert_batch_confirmation.png)

4. 点击**确认**完成批量操作。 

## 结果

- 如导入的Excel文件中的记录已存在，则原纪录的配置将被Excel中的设置替换，即更新已有告警级别设置。
- 如果文件中的记录是新纪录，则自动创建新纪录，即新建告警级别。

