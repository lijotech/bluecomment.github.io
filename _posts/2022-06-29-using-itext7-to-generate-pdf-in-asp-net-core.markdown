---
layout: post
title: "Using iText7 to Generate Pdf in Asp.net Core"
date: 2022-09-29 10:50:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["using itext7 in asp.net", "how to use itext7 in dotnet core", "itext7", "itext7 with dotnet", "common uses of itext7", "how to use itext7 to generate pdf", "how to add image with itext7", "how to add background image in itext7 pdf", "how to add page border in itext7", "how to do cell formatting in itext7", "how to add custom font in itext7", "how to use ByteArrayOutputStream in itext7", "how to give file response in api", "how to use datatable in itext7"]
permalink: /post/using-itext7-to-generate-pdf-in-asp-net-core
---

[iText7](https://itextpdf.com/products/itext-7/itext-7-core "iText7 ") is a powerful library to create customized pdfs. In this article, we will discuss generating pdf using some of the cool features of this library. The main advantage of itext7 is that, it gives fine grain control in the pdf document creation and lot of customization options.

> _Note: The main idea of this post is to make you familiar with the different usage scenarios of the iText7, so we will not discuss much on the dot net core project creation and solution structure. In this solution, we are using the Dependency injection concept, so you are assumed to be familiar with those concepts in advance._

We have created a simple web api project in dot net core. Below are the classes and objects used in the project to demonstrate pdf generation.

*   Service Method class (PdfGenerateService.cs)
*   Utility Class (Utility.cs)
*   FileDownload DTO (FileDownloadDto.cs)

### Prerequisite:

We have to add **itext7** package from Nuget  
![](/assets/img/posts/2022/09/ItextNuget.JPG)

Once the package is added we can see it below in the packages section.

![](/assets/img/posts/2022/09/Itextpackage.JPG)

In the service class, we have added three methods for various types of customization.

```csharp
//Customize a text cell
private static Cell CreateTextCellCustom(
        string text,
        TextAlignment textAlignment,
        VerticalAlignment cellVerticalAlignment,
        float fontsize,
        Color fontColor,
        PdfFont fontType,
        bool isBold = false,
        bool isCellBorder = false,
        bool isCellBackgroundColor = false,
        int rowSpan = 0,
        int colSpan = 0,
        float commonCellPadding = 0,
        float multipliedLeading = 1)
        {

            Cell cell = new Cell(rowSpan, colSpan);
            Paragraph p = new Paragraph();
            p.SetTextAlignment(textAlignment);

            p.Add(new Text(text).SetFont(fontType));
            p.SetFontColor(fontColor).SetFontSize(fontsize);
            p.SetMultipliedLeading(multipliedLeading);
            if (isBold)
                p.SetBold();
            cell.Add(p).SetVerticalAlignment(cellVerticalAlignment).SetPadding(commonCellPadding);
            if (!isCellBorder)
                cell.SetBorder(Border.NO\_BORDER);
            if (isCellBackgroundColor)
            {
                Color customColor = new DeviceRgb(221, 235, 247);
                cell.SetBackgroundColor(customColor);
            }
            return cell;
        }

//Cutomize an image cell
private static Cell CreateImageCell(
        string path,
        VerticalAlignment cellVerticalAlignment,
        int rowSpan = 0,
        int colSpan = 0,
        float percentageImageWidth = 100)
        {
            Image img = new Image(ImageDataFactory.Create(path));
            img.SetWidth(UnitValue.CreatePercentValue(percentageImageWidth));
            Cell cell = new Cell(rowSpan, colSpan).Add(img).SetVerticalAlignment(cellVerticalAlignment);
            cell.SetBorder(Border.NO\_BORDER);
            return cell;
        }

//Customize an empty cell
  public static Cell FormattedEmptyCell(
        Color cellBackgroudColor,
        Color cellBorderColor,
        float cellBorderWidth = 0.6f,
        int rowSpan = 0,
        int colSpan = 0,
        float opacity = 1f)
        {

            Cell cellFirst = new Cell(rowSpan, colSpan);
            cellFirst.SetBorder(Border.NO\_BORDER);
            cellFirst.SetBorderLeft(new SolidBorder(cellBorderColor, cellBorderWidth));
            cellFirst.SetBorderBottom(new SolidBorder(cellBorderColor, cellBorderWidth));
            cellFirst.SetBorderTop(new SolidBorder(cellBorderColor, cellBorderWidth));
            cellFirst.SetBorderRight(new SolidBorder(cellBorderColor, cellBorderWidth));
            cellFirst.SetBackgroundColor(cellBackgroudColor, opacity);
            return cellFirst;
        }
```

Let us discuss different customizations:

1.  **Usage of rowspan and colspan**
    This is similar to rowspan and colspan concept in HTML tables, the way we do it is based on the rows or columns values pass as parameters to the method when creating a cell.  
    
       `Cell cell = new Cell(rowSpan, colSpan);`
    
2.  **Using an image inside a cell**
    In the createImageCell method, we add functionality to add an image to a cell. We have to pass the file path and cell customization values including rowspan, collspan, vertical alignment, and border to the method. The image width percentage is at default 100 and we have the option to resize as well.         

    ```csharp     
    Image img = new Image(ImageDataFactory.Create(path)); 
                img.SetWidth(UnitValue.CreatePercentValue(percentageImageWidth));
                Cell cell = new Cell(rowSpan, colSpan).Add(img).SetVerticalAlignment(cellVerticalAlignment);   
                cell.SetBorder(Border.NO_BORDER);
                return cell;        
    ```   

3.  **Using Background Image on Page**  
    Here we use the image path, fixed positioning to locate the image, and absolute positioning to resize the image on the document.  
    
    ```csharp
    Image imagebackground = new Image(ImageDataFactory.Create(BACKGROUNDIMGPATH));
        imagebackground.SetFixedPosition(1, 0, 60);
        imagebackground.ScaleAbsolute(530, 600);
        doc.Add(imagebackground);
    ```
    
4.  **Adding Background Color and Opacity to Cell**  
    For this, we define a color object using DeviceRgb color values and assign it to the SetBackgroundColor property of the cell. Opacity is optional property so, if we want the cell to be transparent we have to specify a value less than 1 in the opacity field.  
    
    ``` csharp
    Cell cell = new Cell(rowSpan, colSpan);
    Color customColor = new DeviceRgb(221, 235, 247);
    cell.SetBackgroundColor(customColor);
    //if opacity needed
    cell.SetBackgroundColor(customColor, opacity);
    ```
    
5.  **Adding Border Color to Cell**  
    Here we set no border to the cell initially and then customize each edge of the cell using border color and width.
       
    ```csharp
    Cell cellFirst = new Cell(rowSpan, colSpan);
    cellFirst.SetBorder(Border.NO_BORDER);
    cellFirst.SetBorderLeft(new SolidBorder(cellBorderColor, cellBorderWidth));
    cellFirst.SetBorderBottom(new SolidBorder(cellBorderColor, cellBorderWidth));
    cellFirst.SetBorderTop(new SolidBorder(cellBorderColor, cellBorderWidth));
    cellFirst.SetBorderRight(new SolidBorder(cellBorderColor, cellBorderWidth));
    
    Cell Border Color is the Color object we discussed above and cell border width unit is in float ( float cellBorderWidth = 0.6f)  
    ```
    
6.  **Adding page border using PdfCanvas**  
    We use the stroke property to draw lines. Firstly we identify the page to draw using the page number, then we find the page size from the document and define a shape, here we use the rectangle object to draw a page border. Then we allocate some offset from the edge of the page to the rectangle object. Finally, we provide the above properties to Pdf  Canvas and draw rectangle border using a stroke.  

    ```csharp
    PdfPage page = pdfDoc.GetPage(1);
    float width = pdfDoc.GetDefaultPageSize().GetWidth();
    float height = pdfDoc.GetDefaultPageSize().GetHeight();
    Rectangle pageRect = new Rectangle(20, 20, width - 40, height - 40);
    new PdfCanvas(page).SetStrokeColor(CELL\_BORDER\_COLOR).SetLineWidth(0.6f).Rectangle(pageRect).Stroke(); 
    ```

7.  **Formating text inside a cell**  
    We use Paragraph objects for text display, so in the method, we have provided text alignment, font color, font type, font size, bold, and line height  options for formatting. Multiplied leading is used to calculate line-height based on font of text.1 is the default height of font and if we need to reduce or increase we have to specify the value accordingly for _multipliedLeading_ property.  
    
    ```csharp
     Cell cell = new Cell(rowSpan, colSpan);
                Paragraph p = new Paragraph();
                p.SetTextAlignment(textAlignment);
                p.Add(new Text(text).SetFont(fontType));
                p.SetFontColor(fontColor).SetFontSize(fontsize);
                p.SetMultipliedLeading(multipliedLeading);           
                p.SetBold();
    ```

8.  **Formatting cell contents**  
    We can vertically align the contents of cell and provide padding using below options.  
    
    ```csharp
    Cell cell = new Cell(rowSpan, colSpan);
         Paragraph p = new Paragraph();            
         p.Add(new Text(text).SetFont(fontType));           
         cell.Add(p).SetVerticalAlignment(cellVerticalAlignment).SetPadding(commonCellPadding);
    ```

9.  **Adding line break or Empty line**  
    We use paragraph objects to add an empty line to the pdf document as mentioned below.  
    
    `doc.Add(new Paragraph());`
    
10. **Importing Custom Font**   
    We can use font files like (.ttf,.woff) to use custom font in pdf generation. then using pdf font factory we create a pdffont object from font file path. Afterwards use custom font in the text property to display.


    ```csharp
    public static readonly string CUSTOM_FONT_URL = System.IO.Path.Combine(Directory.GetCurrentDirectory(), "assets", "StarellaTattoo.ttf");
    var CUSTOM_FONT = PdfFontFactory.CreateFont(FontProgramFactory.CreateFont(CUSTOM_FONT_URL), PdfEncodings.WINANSI,
                                            PdfFontFactory.EmbeddingStrategy.PREFER_EMBEDDED);
        Paragraph p = new Paragraph();            
        p.Add(new Text(text).SetFont(CUSTOM_FONT));
    ```


11. **Using Datatable to populate Data**  
    Here we use two for populating datable header columns and respectively rows. The table has a percentage array that defines the width of each column, this array count should be the same as the of column count in datatable.  
    
    ```csharp
    int noOfColumns = dataTable.Columns.Count;
    int fixedColumnWidthPercentage = 2;
    var percentageArray = Enumerable.Repeat<float>(fixedColumnWidthPercentage, noOfColumns).ToArray();
    
    Table table = new Table(UnitValue.CreatePercentArray(percentageArray)).UseAllAvailableWidth();
    //Adding column headers
    foreach (DataColumn dataColumn in dataTable.Columns)
    {
        table.AddCell(CreateTextCellCustom(dataColumn.ColumnName, TextAlignment.LEFT, VerticalAlignment.BOTTOM, 8, DEFAULT\_FONT\_COLOR, DEFAULT\_FONT\_TYPE, true, true, true, commonCellPadding: 5));
    }
    //Adding data in each cell.
    foreach (DataRow dataRow in dataTable.Rows)
    {
        foreach (DataColumn dataColumn in dataTable.Columns)
        {
              table.AddCell(CreateTextCellCustom(dataRow\[dataColumn.ColumnName\].ToString(), TextAlignment.LEFT, VerticalAlignment.BOTTOM, 8, DEFAULT\_FONT\_COLOR, DEFAULT\_FONT\_TYPE, false, true, commonCellPadding: 5));
         }
    }
    ```

12. **Using ByteArrayOutputStream**   
    The main purpose of using ByteArrayOutputStream is that it helps to implement an outputstream in which data is written into a byte array, the buffer automatically increases as we write data.  
    
    ```csharp
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    PdfDocument pdfDoc = new PdfDocument(new PdfWriter(baos));
    
    //rest of pdf generation logic
    
    resultFile.Attachment = baos.ToArray();
    ```

13. **Generating response in File format**  
    In the pdf generation service, we use FileDownloadDto to transfer byte array data to the controller. It also has other fields like MimeType and Filename which are used to provide a file response.  
    
    ```csharp
    FileDownloadDto resultFile = new FileDownloadDto
    {
       MimeType = "application/pdf"
    };
    //rest of pdf generation logic
    resultFile.Attachment = baos.ToArray();
    resultFile.FileName = string.Format("Test\_Data\_PDF\_{0}.pdf", System.DateTime.Now.ToString("yyyyMMddHHmmssffff"));
    return Task.FromResult(resultFile);
    
    //controller method give file response
    public async Task<IActionResult> GeneratePdf()
    {            
      var result = await \_pdfGenerateService.GeneratePdf(Utility.GenerateDatatableWithData(4,3));
      return File(result.Attachment, result.MimeType, result.FileName);               
    }
    ```

The full code inside PdfGenerateService class is as below. All the functionalities we have discussed above have been used in the service class. The generated sample pdf can be seen here [Sample Pdf](/assets/img/posts/2022/09/Test_Data_PDF_202207011112499805.pdf "Download Sample Pdf").

```csharp
public class PdfGenerateService : IPdfGenerateService
    {
 
        private static Color DEFAULT_FONT_COLOR = new DeviceRgb(0, 0, 0);
        private static Color CELL_BORDER_COLOR = new DeviceRgb(215, 222, 232);
        private static Color CELL_BACKGROUND_COLOR = new DeviceRgb(238, 232, 213);
        private PdfFont DEFAULT_FONT_TYPE;
        public static readonly string LOGOIMGPATH = System.IO.Path.Combine(Directory.GetCurrentDirectory(), "assets", "logo.jpg");
        public static readonly string BACKGROUNDIMGPATH = System.IO.Path.Combine(Directory.GetCurrentDirectory(), "assets", "background.png");
        public static readonly string CUSTOM_FONT_URL = System.IO.Path.Combine(Directory.GetCurrentDirectory(), "assets", "StarellaTattoo.ttf");
        
         
        public Task<FileDownloadDto> GeneratePdf(DataTable dataTable)
        {
            DEFAULT_FONT_TYPE = PdfFontFactory.CreateFont(StandardFonts.HELVETICA);
            var CUSTOM_FONT = PdfFontFactory.CreateFont(FontProgramFactory.CreateFont(CUSTOM_FONT_URL), PdfEncodings.WINANSI,
                                         PdfFontFactory.EmbeddingStrategy.PREFER_EMBEDDED);
 
            FileDownloadDto resultFile = new FileDownloadDto
            {
                MimeType = "application/pdf"
            };
            int noOfColumns = dataTable.Columns.Count;
            int fixedColumnWidthPercentage = 2;
 
            var percentageArray = Enumerable.Repeat<float>(fixedColumnWidthPercentage, noOfColumns).ToArray();
 
 
 
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            PdfDocument pdfDoc = new PdfDocument(new PdfWriter(baos));
            Document doc = new Document(pdfDoc);
 
            //code to add background Image
            Image imagebackground = new Image(ImageDataFactory.Create(BACKGROUNDIMGPATH));
            imagebackground.SetFixedPosition(1, 0, 60);
            imagebackground.ScaleAbsolute(530, 600);
            doc.Add(imagebackground);
 
            //define the table and columns for header 
            Table headerTable = new Table(UnitValue.CreatePercentArray(new float[] { 2, 3, 2 })).UseAllAvailableWidth();
 
            //adding logo Title and other fields in head section
            headerTable.AddCell(CreateImageCell(LOGOIMGPATH, VerticalAlignment.BOTTOM, 4, 0, 50));
            headerTable.AddCell(CreateTextCellCustom("Main Title with rowspan", TextAlignment.CENTER, VerticalAlignment.BOTTOM, 10, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, isBold: true, isCellBorder: false, isCellBackgroundColor: false, rowSpan: 4));
            headerTable.AddCell(CreateTextCellCustom($"Date :{System.DateTime.Today.ToString("dd-MMM-yyyy")}", TextAlignment.LEFT, VerticalAlignment.BOTTOM, 8, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, multipliedLeading: 1.5f));
            headerTable.AddCell(CreateTextCellCustom($"Time : {System.DateTime.Now.ToString("HH:mm")}", TextAlignment.LEFT, VerticalAlignment.BOTTOM, 8, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, multipliedLeading: 1.5f));
            headerTable.AddCell(CreateTextCellCustom("Report generated by : UserName", TextAlignment.LEFT, VerticalAlignment.BOTTOM, 8, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, multipliedLeading: 1.5f));
            doc.Add(headerTable);
 
            //define table columns for the data
            Table table = new Table(UnitValue.CreatePercentArray(percentageArray)).UseAllAvailableWidth();
 
            table.AddCell(CreateTextCellCustom("Sub title with colspan", TextAlignment.CENTER, VerticalAlignment.BOTTOM, 9, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, true, false, false, 0, 7, 5, 2f));
            //Adding column headers
            foreach (DataColumn dataColumn in dataTable.Columns)
            {
                table.AddCell(CreateTextCellCustom(dataColumn.ColumnName, TextAlignment.LEFT, VerticalAlignment.BOTTOM, 8, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, true, true, true, commonCellPadding: 5));
            }
 
            //Adding data in each cell.
            foreach (DataRow dataRow in dataTable.Rows)
            {
                foreach (DataColumn dataColumn in dataTable.Columns)
                {
                    table.AddCell(CreateTextCellCustom(dataRow[dataColumn.ColumnName].ToString(), TextAlignment.LEFT, VerticalAlignment.BOTTOM, 8, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, false, true, commonCellPadding: 5));
                }
            }
 
            doc.Add(table);
            doc.Add(new Paragraph());
            doc.Add(new Paragraph());
            doc.Add(new Paragraph());
 
 
            Table tableNext = new Table(UnitValue.CreatePercentArray(new float[] { 10 }));
            tableNext.SetWidth(UnitValue.CreatePercentValue(100));
            tableNext.SetFixedLayout();
 
            tableNext.AddCell(CreateTextCellCustom("Sub title with Custom Font", TextAlignment.CENTER, VerticalAlignment.BOTTOM, 20, DEFAULT_FONT_COLOR, CUSTOM_FONT, true, false, false, 0, 0, 5,2f));
            Cell mainCell = FormattedEmptyCell(CELL_BACKGROUND_COLOR, CELL_BORDER_COLOR,opacity:0.5f);
                Table innerTable = new Table(UnitValue.CreatePercentArray(new float[] { 3, 7,2 }));
                innerTable.SetWidth(UnitValue.CreatePercentValue(100));
                innerTable.SetFixedLayout();
 
 
                //table header styles
                innerTable.AddCell(CreateTextCellCustom("MAIN ITEM",
                TextAlignment.LEFT, VerticalAlignment.BOTTOM, 9, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, true, false, commonCellPadding: 5));
 
                innerTable.AddCell(CreateTextCellCustom("DESCRIPTION",
                TextAlignment.LEFT, VerticalAlignment.BOTTOM, 9, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, true, false, commonCellPadding: 5));
 
                innerTable.AddCell(CreateTextCellCustom("AMOUNT",
                TextAlignment.RIGHT, VerticalAlignment.BOTTOM, 9, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, true, false, commonCellPadding: 5));
 
                //table data values
                innerTable.AddCell(CreateTextCellCustom("Item name",
                TextAlignment.LEFT, VerticalAlignment.BOTTOM, 8, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, true, false, commonCellPadding: 5));
 
                innerTable.AddCell(CreateTextCellCustom("Item description in detail",
                TextAlignment.LEFT, VerticalAlignment.BOTTOM, 8, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, true, false, commonCellPadding: 5));
 
                innerTable.AddCell(CreateTextCellCustom("23.00",
                TextAlignment.RIGHT, VerticalAlignment.BOTTOM, 8, DEFAULT_FONT_COLOR, DEFAULT_FONT_TYPE, true, false, commonCellPadding: 5));
 
 
            mainCell.Add(innerTable);
            tableNext.AddCell(mainCell);
            doc.Add(tableNext);
 
            PdfPage page = pdfDoc.GetPage(1);
            float width = pdfDoc.GetDefaultPageSize().GetWidth();
            float height = pdfDoc.GetDefaultPageSize().GetHeight();
            Rectangle pageRect = new Rectangle(20, 20, width - 40, height - 40);
            new PdfCanvas(page).SetStrokeColor(CELL_BORDER_COLOR).SetLineWidth(0.6f).Rectangle(pageRect).Stroke();          
 
            doc.Close();
 
            resultFile.Attachment = baos.ToArray();
            resultFile.FileName = string.Format("Test_Data_PDF_{0}.pdf", System.DateTime.Now.ToString("yyyyMMddHHmmssffff"));
            return Task.FromResult(resultFile);
        }
 
 
        private static Cell CreateTextCellCustom(
        string text,
        TextAlignment textAlignment,
        VerticalAlignment cellVerticalAlignment,
        float fontsize,
        Color fontColor,
        PdfFont fontType,
        bool isBold = false,
        bool isCellBorder = false,
        bool isCellBackgroundColor = false,
        int rowSpan = 0,
        int colSpan = 0,
        float commonCellPadding = 0,
        float multipliedLeading = 1)
        {
 
            Cell cell = new Cell(rowSpan, colSpan);
            Paragraph p = new Paragraph();
            p.SetTextAlignment(textAlignment);
 
            p.Add(new Text(text).SetFont(fontType));
            p.SetFontColor(fontColor).SetFontSize(fontsize);
            p.SetMultipliedLeading(multipliedLeading);
            if (isBold)
                p.SetBold();
            cell.Add(p).SetVerticalAlignment(cellVerticalAlignment).SetPadding(commonCellPadding);
            if (!isCellBorder)
                cell.SetBorder(Border.NO_BORDER);
            if (isCellBackgroundColor)
            {
                Color customColor = new DeviceRgb(221, 235, 247);
                cell.SetBackgroundColor(customColor);
            }
            return cell;
        }
 
        private static Cell CreateImageCell(
        string path,
        VerticalAlignment cellVerticalAlignment,
        int rowSpan = 0,
        int colSpan = 0,
        float percentageImageWidth = 100)
        {
            Image img = new Image(ImageDataFactory.Create(path));
            img.SetWidth(UnitValue.CreatePercentValue(percentageImageWidth));
            Cell cell = new Cell(rowSpan, colSpan).Add(img).SetVerticalAlignment(cellVerticalAlignment);
            cell.SetBorder(Border.NO_BORDER);
            return cell;
        }
 
        public static Cell FormattedEmptyCell(
        Color cellBackgroudColor,
        Color cellBorderColor,
        float cellBorderWidth = 0.6f,
        int rowSpan = 0,
        int colSpan = 0,
        float opacity = 1f)
        {
 
            Cell cellFirst = new Cell(rowSpan, colSpan);
            cellFirst.SetBorder(Border.NO_BORDER);
            cellFirst.SetBorderLeft(new SolidBorder(cellBorderColor, cellBorderWidth));
            cellFirst.SetBorderBottom(new SolidBorder(cellBorderColor, cellBorderWidth));
            cellFirst.SetBorderTop(new SolidBorder(cellBorderColor, cellBorderWidth));
            cellFirst.SetBorderRight(new SolidBorder(cellBorderColor, cellBorderWidth));
            cellFirst.SetBackgroundColor(cellBackgroudColor, opacity);
            return cellFirst;
        } 
    }
```
Overall we have discussed some of the daily use features and customization possible with iText7 library. The scope of the library is much more than what we discussed above, however above mentioned tips will be very handy while you are developing pdf for some applications.

The complete source code of the project is available [here](https://github.com/lijotech/PdfGenerateDemo "Pdf Generate Demo").

# Hope you enjoy the article. Kindly share it on your social media profile if you find this helpful and don't forget to drop in your comments.