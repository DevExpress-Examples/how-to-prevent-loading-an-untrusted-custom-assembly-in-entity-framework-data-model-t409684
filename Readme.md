# How to prevent loading an untrusted custom assembly in Entity Framework data model


This example addresses securirty considerations that are specific to mail merge using data from Entity Framework data models contained in a compiled assembly.<br>Before loading the data assembly you should have an option to perform a path check to ensure that an assembly is obtained from a trusted source and that the path length is within valid limits.<br><br>When this example runs, it asks the user for permission to load the assembly. The first request is initiated by the <a href="http://help.devexpress.com/#CoreLibraries/DevExpressSpreadsheetIWorkbook_GenerateMailMergeDocumentstopic">GenerateMailMergeDocuments</a> method, the second request occurs when the Spreadsheet Control attempts to populate its Field List. Each request fires the <a href="http://help.devexpress.com/#WindowsForms/DevExpressXtraSpreadsheetSpreadsheetControl_CustomAssemblyLoadingtopic">CustomAssemblyLoading</a> event. The event handler asks the end-user for the permission and prompts whether the decision is final. If the decision is not final, the <a href="http://help.devexpress.com/#CoreLibraries/clsDevExpressXtraSpreadsheetServicesICustomAssemblyLoadingNotificationServicetopic">ICustomAssemblyLoadingNotificationService</a> is used to grant or deny the permission to load the assembly.


<h3>Description</h3>

The <a href="http://help.devexpress.com/#CoreLibraries/DevExpressXtraSpreadsheetSpreadsheetDataSourceLoadingOptions_CustomAssemblyBehaviortopic">SpreadsheetControl.Options.DataSourceLoading.CustomAssemblyBehavior</a>&nbsp;option allows you to specify whether to load custom data assemblies, not load them or perform additional steps to decide for each file. An attempt to load the Entity Framework data model fires the <a href="http://help.devexpress.com/#WindowsForms/DevExpressXtraSpreadsheetSpreadsheetControl_CustomAssemblyLoadingtopic">SpreadsheetControl.CustomAssemblyLoading</a>&nbsp;event (if this event has a subscriber). If there is no subscription or the <strong>e.Handled</strong>&nbsp;property is set to <strong>false</strong>, the <strong>RequestApproval</strong>&nbsp;method of the service which implements the&nbsp;<a href="http://help.devexpress.com/#CoreLibraries/clsDevExpressXtraSpreadsheetServicesICustomAssemblyLoadingNotificationServicetopic">ICustomAssemblyLoadingNotificationService</a>&nbsp;interface is&nbsp;called. If it returns <strong>true,</strong> the assembly is loaded.<br>&nbsp;<br>When the <a href="http://help.devexpress.com/#WindowsForms/CustomDocument17836">Data Source Wizard</a> is used to bind to the&nbsp;Entity Framework data model, the <a href="http://help.devexpress.com/#WindowsForms/DevExpressXtraSpreadsheetSpreadsheetDataSourceWizardOptions_CustomAssemblyBehaviortopic">SpreadsheetControl.Options.DataSourceWizard.CustomAssemblyBehavior</a><strong>&nbsp;</strong>setting determines whether the Wizard allows loading a custom assembly. &nbsp;If the setting has the <strong>DevExpress.XtraSpreadsheet.SpreadsheetCustomAssemblyBehavior.Prompt </strong>value (default), the&nbsp;<strong>RequestApproval</strong>&nbsp;method of the service which implements the&nbsp;<a href="http://help.devexpress.com/#WindowsForms/clsDevExpressDataAccessUIWizardServicesICustomAssemblyPathNotifiertopic">ICustomAssemblyPathNotifier</a>&nbsp;interface is&nbsp;called. If it returns <strong>true,</strong> the assembly is loaded.<br><br>To demonstrate the behavior described above, run this example. On the first run it creates a complied assembly containing Entity Framework data model and performs mail merge with these data.&nbsp;The&nbsp;<strong>CustomAssemblyLoading</strong>&nbsp;event handler prompts you for the<strong> e.Cancel</strong> and <strong>e.Handler</strong> values. If your answer sets the e.Handler value to&nbsp;<strong>false,</strong>&nbsp;the decision is passed&nbsp;to the custom service which displays a dialog&nbsp;for the final answer.<br>After starting the application explore the Data Source Wizard behavior. Click <strong>Add New Data Source</strong>, select <strong>Entity Framework</strong> and&nbsp;browse for the&nbsp;<em>EFDataModel.dll</em> assembly located at the application executable path. When you click <strong>Open</strong> to load this assembly, the service prompts you for the final decision. If you click OK, provide the Wizard with the following connection string:<br>
<code lang="cs">Data Source=(LocalDB)\MSSQLLocalDB;attachdbfilename=|DataDirectory|\Contacts.mdf;integrated security=True;</code>

<br/>


