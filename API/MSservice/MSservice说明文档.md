# 背景
 * 该项目用于给第三方机构提供获取睡眠报告的api。具体有哪些api可以参见   MSservice兆观睡眠服务项目说明v1.3_来邦.docx 文件。
 * 项目核心文件routes/MSservice.js
 * 对应leancloud的MSservice项目
 * QueryListHistory 该表用于记录 报告列表查询方法reportList的调用次数。
 * QueryReportHistory 该表用于记录 报告详细查询方法reportDetail的调用次数。
 * role 该表用于记录各种类型的用户。
 <table style="margin:0 auto;width:100%;" cellspacing="0" cellpadding="0">
	<tr>
		<td>字段</td>
		<td>意义</td>
		<td>数据类型</td>
		<td>备注</td>
	</tr>
	<tr>
		<td>role</td>
		<td>有权限的http接口</td>
		<td>Array</td>
		<td>未填写，则该用户无改接口权限</td>
	</tr>
	<tr>
		<td>description</td>
		<td>描述</td>
		<td>String</td>
		<td></td>
	</tr>
	<tr>
		<td>appName</td>
		<td>应用名</td>
		<td>String</td>
		<td></td>
	</tr>
	<tr>
		<td>key</td>
		<td></td>
		<td>String</td>
		<td>手动生成的一个随机码，做身份验证记录。经过md5加密</td>
	</tr>
	<tr>
		<td>id</td>
		<td></td>
		<td>String</td>
		<td>手动生成的一个随机码，做身份验证记录。经过md5加密</td>
	</tr>
	<tr>
		<td>appkey</td>
		<td>lean的对应项目的appkey</td>
		<td>String</td>
		<td></td>
	</tr>
	<tr>
		<td>appid</td>
		<td>同上</td>
		<td>String</td>
		<td></td>
	</tr>
	<tr>
		<td>token</td>
		<td>完成身份验证后，后续登录的身份标志</td>
		<td>String</td>
		<td></td>
	</tr>
</table>
* 通过session记录会话，保存身份信息。