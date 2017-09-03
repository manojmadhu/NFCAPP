
import android.app.Activity;
import android.app.PendingIntent;
import android.content.Intent;
import android.content.IntentFilter;
import android.nfc.NfcAdapter;
import android.nfc.Tag;
import android.nfc.tech.NfcA;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.util.Log;
import android.view.Menu;
import android.widget.Toast;

import com.samsung.android.sdk.pass.SpassInvalidStateException;

import java.io.IOException;
import java.math.BigInteger;

/**
 * Created by manojm on 9/3/2017.
 * THIS CLASS CREATED FOR READING NFC TAGS.
 * HAVE TO ADD THIS PERMISSION TO MANIFEST
 *  --    <uses-permission android:name="android.permission.NFC"/>
    --   <uses-feature android:name="android.hardware.nfc" android:required="true"/>
 */

public class NFC_Class extends Activity{
    private Menu menu;
    Tag myTag;
    private static String TAG = "NFC_Class";
    NfcAdapter nfcAdapter;
    private  String NFCID = "0";


    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        nfcAdapter=NfcAdapter.getDefaultAdapter(this);
    }


    void resolveIntent(Intent intent) {
        try {


            /** Reading NFC TAG **/

            Log.i(TAG, "resolving intent");
            Tag tag = intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);
            String [] taglist=tag.getTechList();

            byte[] UID=tag.getId();
            NFCID = ByteArrayToHexString(UID);//Get NFC ID
            BigInteger bi=new BigInteger(UID);
            NFCID = bi.toString();


            //<<>>#Begin TAG Data deep reader region
            if (tag != null) {
                Log.i(TAG, "found a tag");

                NfcA nfca = NfcA.get(tag);
                String atqa = ByteArrayToHexString(nfca.getAtqa());
                Short sak = nfca.getSak();
                byte[] bArray = new byte[8];
                byte[] res;
                byte[] cmdWake = new byte[]{
                        (byte) 0x26
                };

                bArray[0] = 0x26;
                try {
                    nfca.connect();
                    Log.i(TAG, "connected NFCA tag");
                    res = nfca.transceive(cmdWake);
                    Log.i(TAG, "got response: " + res.toString());

                } catch (IOException e) {
                    Log.e(TAG, "caught IO exception", e);
                } finally {
                    try {
                        nfca.close();
                    } catch (IOException e) {
                        Log.e(TAG, "caught IO exception", e);
                    }
                }
                //<<>>#End TAG Data deep reader region
            }
        }catch (Exception ex){

        }
    }


    // Converting byte[] to hex string:
    private String ByteArrayToHexString(byte [] inarray) {
        int i, j, in;
        String [] hex = {"0","1","2","3","4","5","6","7","8","9","A","B","C","D","E","F"};
        String out= "";
        for(j = 0 ; j < inarray.length ; ++j)
        {
            in = (int) inarray[j] & 0xff;
            i = (in >> 4) & 0x0f;
            out += hex[i];
            i = in & 0x0f;
            out += hex[i];
        }
        return out;
    }



    @Override
    protected void onNewIntent(Intent intent) {
        Toast.makeText(this, "Tag Detect", Toast.LENGTH_SHORT).show();
        resolveIntent(intent);
        super.onNewIntent(intent);
    }

    @Override
    protected void onPause() {
        nfcAdapter.disableForegroundDispatch(this);
        super.onPause();

    }

    @Override
    public void onResume() {
        Intent intent = new Intent(this,MainActivity.class);
        intent.addFlags(Intent.FLAG_RECEIVER_REPLACE_PENDING);
        PendingIntent pendingIntent=PendingIntent.getActivity(this,0,intent,0);
        IntentFilter[] intentFilters=new IntentFilter[] {};
        nfcAdapter.enableForegroundDispatch(this,pendingIntent,intentFilters,null);
        super.onResume();

    }

}
