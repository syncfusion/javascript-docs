---
layout: post
title: Exporting with PivotClient widget for Syncfusion Essential JS
description: export
platform: js
control: PivotClient
documentation: ug
api: /api/js/ejpivotclient
---

# Exporting

The pivot chart and the pivot grid in the pivot client widget can be exported to Microsoft Excel, Microsoft Word, and PDF documents by clicking the respective toolbar icons.

![Exporting icons in JavaScript pivot client control](Export_images/exporting-icons.png)

Exporting feature provides an option that allows you to export the pivot chart or pivot grid or both with the use of the [`clientExportMode`](/api/js/ejpivotclient#members:clientExportMode) property.

The [`clientExportMode`](/api/js/ejpivotclient#members:clientExportMode) property takes any one of the following values:

* **ChartAndGrid** – Exports both the pivot chart and pivot grid controls. This mode is the default mode.
* **ChartOnly** – Exports the pivot chart control alone.
* **GridOnly** – Exports the pivot grid control alone.

## JSON export

I> By default, exporting is done with the use of JSON records maintained in the client-side for both client and server modes.

To perform exporting with the use of a custom service method, the service containing the exporting method is hosted and its link is given in a URL as shown below. Without giving any value to the 'url' property, it takes the default exporting service link.

{% highlight javascript %}

        $("#PivotClient1").ejPivotClient({
            //...
            beforeExport:"Export",
            clientExportMode: ej.PivotClient.ClientExportMode.ChartAndGrid
        });

        function Export(args) {
            args.url = "https://js.syncfusion.com/ejservices/api/PivotClient/Olap/Export";//Exporting url can be modified here
        }
{% endhighlight %}

### Customize the export document name

The name of the document can be customized. The following code sample illustrates this process:

{% highlight javascript %}

        $("#PivotClient1").ejPivotClient({
            //...
            beforeExport:"Export",
            clientExportMode: ej.PivotClient.ClientExportMode.ChartAndGrid
        });

        function Export(args) {
            args.url = "https://js.syncfusion.com/ejservices/api/PivotClient/Olap/Export";
            args.fileName="File name is customized here";
        }
{% endhighlight %}

## Pivot engine export

I> This feature is applicable only at the server mode operation.

To perform exporting with the use of the pivot engine available in server-side, the 'exportMode' property obtained in the “beforeExport” event is set to "ej.PivotClient.ExportMode.PivotEngine" as shown below:

{% highlight javascript %}

        $("#PivotClient1").ejPivotClient({
            url: "/OlapService",
            beforeExport:"Export"
        });

        function Export(args) {
            args.exportMode = ej.PivotClient.ExportMode.PivotEngine;
        }

 {% endhighlight %}

For WebAPI controller, the following method should be added to perform exporting with the pivot engine:

{% highlight c# %}

    [System.Web.Http.ActionName("Export")]
    [System.Web.Http.HttpPost]
    public void Export() {
        string args = HttpContext.Current.Request.Form.GetValues(0)[0];
        OlapDataManager DataManager = new OlapDataManager(connectionString);
        string fileName = "Sample";
        olapClientHelper.ExportOlapClient(DataManager, args, fileName, System.Web.HttpContext.Current.Response);
    }

{% endhighlight %}

For WCF service, the following service method should be added to perform exporting with the pivot engine:

{% highlight c# %}

    public void Export(Stream stream) {
        System.IO.StreamReader sReader = new System.IO.StreamReader(stream);
        string args = System.Web.HttpContext.Current.Server.UrlDecode(sReader.ReadToEnd());
        OlapDataManager DataManager = new OlapDataManager(connectionString);
        string fileName = "OlapClient";
        olapClientHelper.ExportOlapClient(DataManager, args, fileName,
            System.Web.HttpContext.Current.Response);
    }

{% endhighlight %}

### File format selection

I> This option is applicable only for the pivot client when exporting to Excel document.

You can set the option for exporting the control to Excel document either in *.xls* or *.xlsx* format, using the `fileFormat` property in the `beforeExport` event.

N> By default, the excel document will be exported to ".xls" format using the pivot engine export.

{% highlight javascript %}

        $("#PivotClient1").ejPivotClient({
            url: "/OlapService",
            beforeExport:"Export"
        });

        function Export(args) {
            args.exportMode = ej.PivotClient.ExportMode.PivotEngine;
            args.fileFormat = ".xlsx"; //you can set the excel sheet format here
        }

 {% endhighlight %}

### Customize the export document name

The name of the document can be customized in the method of the WebAPI controller. The following code sample illustrates this process:

{% highlight c# %}

    [System.Web.Http.ActionName("Export")]
    [System.Web.Http.HttpPost]
    public void Export() {
        string args = HttpContext.Current.Request.Form.GetValues(0)[0];
        OlapDataManager DataManager = new OlapDataManager(connectionString);
        string fileName = " File name is customized here ";
        olapClientHelper.ExportOlapClient(DataManager, args, fileName, System.Web.HttpContext.Current.Response);
    }

{% endhighlight %}

For customizing a name in the WCF Service, the following code snippet is used:

{% highlight c# %}

    public void Export(Stream stream) {
        System.IO.StreamReader sReader = new System.IO.StreamReader(stream);
        string args = System.Web.HttpContext.Current.Server.UrlDecode(sReader.ReadToEnd()).Remove(0, 5);
        OlapDataManager DataManager = new OlapDataManager(connectionString);
        string fileName = " File name is customized here ";
        olapClientHelper.ExportOlapClient(DataManager, args, fileName, System.Web.HttpContext.Current.Response);
    }

{% endhighlight %}

## Pivot chart - Exporting format

I> This option is applicable only for the pivot chart in the pivot client specifically when exporting to Excel document.

You can set an option to export pivot chart to an Excel document, and you can export it as either an image or pivot chart format itself by setting the Boolean property `exportChartAsImage` in the `beforeExport` event.

N> By default, the pivot chart will be exported to image format in the Excel document.

{% highlight javascript %}

        $("#PivotClient1").ejPivotClient({
            //..
            beforeExport:"Export",
            clientExportMode: ej.PivotClient.ClientExportMode.ChartOnly
        });

        function Exporting(args) {
            args.exportChartAsImage = false; //You can set the chart format here
        }

 {% endhighlight %}

The following screenshot shows the control exported to an Excel document showing its own format (pivoting chart).

![Exporting format in JavaScript pivot client control](Export_images/Export_ExcelChartClient.png)

## Exporting customization

You can add title and description to the exporting document by using the title and description properties respectively obtained in the `beforeExport` event. Similarly, you can enable or disable styling on the exported document by using the `exportWithStyle` property.

{% highlight html %}
<html>
    //...
<body>
    //...
    <div id="PivotClient1"></div>

    <script type="text/javascript">
        $(function () {
             $("#PivotClient1").ejPivotClient({
                //...
                beforeExport:"Exporting",
                clientExportMode: ej.PivotClient.ClientExportMode.ChartAndGrid
             });
        });
        function Exporting(args) {
            //ClientMode export
            args.url = "https://js.syncfusion.com/ejservices/api/PivotClient/Olap/Export";
            //PivotEngine Export
            args.exportMode = ej.PivotClient.ExportMode.PivotEngine;

            args.title = "PivotClient";
            args.description = "Visualizes both OLAP and Relational datasource in tabular and graphical formats";
			args.exportWithStyle = true;   // by default it sets as true. It improves performance on exporting huge data when it sets as false.
        }
    </script>
</body>
</html>

{% endhighlight %}

You can also edit the exporting document by using a server-side event for the required exporting option.

{% highlight c# %}

//...
using Syncfusion.EJ.Export;
using Syncfusion.Compression.Base;
using Syncfusion.XlsIO;
using Syncfusion.DocIO.Base;
using Syncfusion.Pdf.Base;

//Following service method needs to be added in WebAPI for JSON export.

[System.Web.Http.ActionName("Export")]
[System.Web.Http.HttpPost]
public void Export()
{
    PivotClientExport pivotClient = new PivotClientExport();
    pivotClient.ExcelExport += pivotClient_ExcelExport;
    pivotClient.WordExport += pivotClient_WordExport;
    pivotClient.AddPDFHeaderFooter += pivotClient_AddPDFHeaderFooter;
    pivotClient.PDFExport += pivotClient_PDFExport;
    string args = HttpContext.Current.Request.Form.GetValues(0)[0];
    pivotClient.ExportPivotClient(string.Empty, args, HttpContext.Current.Response);
}

void pivotClient_PDFExport(object sender, Syncfusion.Pdf.PdfDocument pdfDoc)
{
    //You can customize exporting document here.
}

void pivotClient_AddPDFHeaderFooter(object sender, Syncfusion.Pdf.PdfDocument pdfDoc)
{
    //You can add header/footer information to the PDF document.
}

void pivotClient_WordExport(object sender, Syncfusion.DocIO.DLS.WordDocument document)
{
    //You can customize exporting document here.
}

void pivotClient_ExcelExport(object sender, Syncfusion.XlsIO.IWorkbook workBook)
{
    //You can customize exporting document here.
}

//Following service method needs to be added in WCF/WebAPI for PivotEngine export.
[System.Web.Http.ActionName("ExportOlapClient")]
[System.Web.Http.HttpPost]
public void ExportOlapClient()
{
    string args = HttpContext.Current.Request.Form.GetValues(0)[0];
    OlapDataManager DataManager = new OlapDataManager(connectionString);
    olapClientHelper.ExcelExport += olapClientHelper_ExcelExport;
    olapClientHelper.WordExport += olapClientHelper_WordExport;
    olapClientHelper.AddPDFHeaderFooter += olapClientHelper_AddPDFHeaderFooter;
    olapClientHelper.PDFExport += olapClientHelper_PDFExport;
    string fileName = "Sample";
    olapClientHelper.ExportPivotClient(DataManager, args, fileName, System.Web.HttpContext.Current.Response);
}

void olapClientHelper_PDFExport(object sender, Syncfusion.Pdf.PdfDocument pdfDoc)
{
    //You can customize exporting document here.
}

void olapClientHelper_AddPDFHeaderFooter(object sender, Syncfusion.Pdf.PdfDocument pdfDoc)
{
    //You can add header/footer information to the PDF document.
}

void olapClientHelper_WordExport(object sender, Syncfusion.DocIO.DLS.WordDocument document)
{
    //You can customize exporting document here.
}

void olapClientHelper_ExcelExport(object sender, Syncfusion.XlsIO.IWorkbook workBook)
{
    //You can customize exporting document here.
}

{% endhighlight %}

### Exporting complete data on Paging

When paging is enabled, you can export the complete data by enabling the [enableCompleteDataExport](/api/js/ejpivotclient#members:enablecompletedataexport) property. It is supported in both types of JSON and PivotEngine export and it is applicable for all kinds of exporting formats available in PivotClient.

{% highlight html %}
<html>
    <div id="PivotClient1"></div>
    //...
    <script type="text/javascript">
        $(function () {
            $("#PivotClient1").ejPivotClient({
                //...
                enableCompleteDataExport: true
            });
        });
    //...
    </script>
</html>

{% endhighlight %}

The following screenshot shows the pivot grid and pivot chart controls exported to an Excel document:

![Excel exporting in JavaScript pivot client control](Export_images/excel-export.png)

The following screenshot shows the pivot grid and pivot chart controls exported to a Word document:

![Word exporting in JavaScript pivot client control](Export_images/Word-Export.png)

The following screenshot shows the pivot grid and pivot chart controls exported to a PDF document:

![PDF exporting in JavaScript pivot client control](Export_images/Pdf-Export.png)