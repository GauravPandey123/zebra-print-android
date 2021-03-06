# zebra-print-android
An Android application library which provides a one-shot intent for connecting to a Zebra printer and providing a ZPL file to be printed.

The library doesn't provide a UI for providing arguments to the template provided, but rather is intended to streamline the process for providing a .ZPL file along with a set of variable arguments provided by an app and having them be printed on a Zebra device over bluetooth.

## Usage

To integrate the library into your app, simply download or compile the .aar library file generated by this Android Project and call out to the library Activity using the format provided in the example below

The library will provide a full UI for scanning for available bluetooth devices, connecting to a device, and printing provided templates.

Unfortunately no web resource is available over Maven or other online dependency managers at this time.

## Example

The following example code prints out a label from a presumed ZPL file **label.zpl** which contains two arguments, a url encoded in a barcode and a string description that are provided by the host application. The two arguments are presumed to be **Variable Text** components in the label, and their **Prompt Text** will be used as the key.


```
    private void printZplTemplate(String barcodeData, String textData){
        String zebraTemplateFilepath = "/sdcard/download/test.zpl";
        
        Intent i = new Intent("com.dimagi.android.zebraprinttool.action.PrintTemplate");
        
        i.putExtra("zebra:template_file_path", zebraTemplateFilepath);
        
        Bundle labelVariableArguments = new Bundle();
        labelVariableArguments.putString("barcode_data", barcodeData);
        labelVariableArguments.putString("text_data", textData);
        i.putExtras(labelVariableArguments);
        
        this.startActivityForResult(i, ZEBRA_CALLOUT);
    }
```

## Limitations 
 * Only supports Bluetooth print devices
 * ZPL Templates must be provided as filesystem paths
 * [Presumed limitation of Zebra API] Labels only seem to support using the **printer settings** for sizing and spacing, encoding these values in the ZPL template results in a blank print.
 * [Pending] Reasonable feedback UI during print execution
