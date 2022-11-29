### 组织结构导入

第一次管理组织结构，建议采用导入 Excel 文件的方式

Excel 文件必须是 xlsx 新格式。
![companya_orgchart](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/companya_orgchart.png)

**XLSX 文件模版**

[xls_orgchart](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/CompanyA.OrgChart.xlsx.png)

右键点击上面连接，去掉.png 后缀，保存为.xlsx 后缀文件

**栏位说明**

第一栏： 第一栏是组织结构编码，root 为顶级结构，必须是 root，root 下面的组织代码长度必须是 5 位的倍数，例如，B0001 为 root 下的一级部门， B000100001， B000100002 为 B0001 下面的两个二级部门

第二栏： 第二栏为公司名称、部门名称、或员工名称。 如第一栏为 root，则其第二栏应为公司的名称； 如第一栏为 5 位数倍数长度的部门代码，第三栏后为空，则第二栏是部门的名称；
如第一栏为 5 位数倍数长度的部门代码，第三栏为邮箱，则第二栏是该部门下一个员工的名称，与第三栏的邮箱地址对应；

第三栏： 只有当当前行为员工时，第三栏是员工的邮箱地址；

第四栏： 第四栏只对员工行有作用，表示该员工的角色，多个角色用英文分号分隔；

管理员可多次导入组织结构文件，覆盖之前的组织结构。

导入组织结构时，须输入“新员工缺省密码”，当 XLSX 文件中存在新员工时，为新员工提供缺省密码。用户使用缺省密码登陆以后，可自行修改。

### 添加或修改员工信息

在第一次导入以后，单独添加少量员工信息，可直接录入

![update_employee](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/update_employee.png)

第一栏为员工所在的部门代码，第二栏为员工姓名，第三栏为员工邮箱。

先从组织结构中点选一个员工，然后员工的信息将被自动填写到上图栏位中。

### 删除、拷贝或移动员工部门

![move_employee](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/move_employee.png)

从组织结构中点选员工后，在上图中选择新的部门，然后点“删除本条”则从其当前部门中移除，点“拷贝到”，则将该员工拷贝到下拉列表中所选的新部门，点“移动到”，将该员工从其原部门移动到下拉列表中所选的新部门。

### 新建、修改、删除部门

![update_ou](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/update_ou.png)

### 导出组织结构

导出组织结构到 XLSX 文件，修改后，可重新上传。
