Read More document [Here](https://github.com/ttpro1995/Android_ZBar_QRcodeScanner/blob/master/_DOCUMENT/Android_ZBar_QRcodeScanner%20document.pdf)




Zbar library
https://github.com/dm77/barcodescanner


###1/ in gradle, to compile library, add 
compile 'me.dm7.barcodescanner:zbar:1.8' 

###2/ In manifest, add some permission
<uses-permission android:name="android.permission.CAMERA" />
<permission android:name="android.permission.FLASHLIGHT"
    android:permissionGroup="android.permission-group.HARDWARE_CONTROLS"
    android:protectionLevel="normal" />
###3/ Declare some global variable in activity
FrameLayout frameLayout;// a placeholder for code scanner preview
ZBarScannerView myScannerView;// scanner view which we will add into frameLayout
ZBarScannerView.ResultHandler resultHandler;//result handler for myScannerView

###4/  In OnCreate of the activity
//TODO: init view and add view to layout
myScannerView = new ZBarScannerView(MainActivity.this);
frameLayout.addView(myScannerView);
//TODO: init the result handler 
resultHandler = new ZBarScannerView.ResultHandler() {
    @Override
    public void handleResult(Result result) {
        // Do something with the result here
        Log.v(TAG, result.getContents()); // Prints scan results
        Log.v(TAG, result.getBarcodeFormat().getName()); // Prints the scan format (qrcode, pdf417 etc.)
        //Result show here
        ShowResultDialog(result.getContents());
    }
};

###5/ In OnResume of the activity 
//TODO: SET Result Handle Here
myScannerView.setResultHandler(resultHandler);
//TODO Start camera
myScannerView.startCamera();
//TODO Set flash and autofocus
myScannerView.setFlash(true);
myScannerView.setAutoFocus(true);

###6/ In OnPause of the activity 
super.onPause();
myScannerView.stopCamera();           // Stop camera on pause

###7/ ShowResultDialog
Just display a dialog what it got from code 
private void ShowResultDialog(String msg){
    AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
    builder.setMessage(msg);
    builder.setPositiveButton("Ok", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialog, int which) {
            MainActivity.this.onResume();
        }
    });
   Dialog dialog = builder.create();
    dialog.show();
}
