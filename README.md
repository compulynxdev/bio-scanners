# bio-scanners

## Compulynx FingerSdk

You will need these libs for fingerprint

1. FingerprintModule.aar
2. opencv_411-release.aar
3. CompulynxAirSnap.aar


Face capture libs

1. FaceCaptureModule.aar
2. AirsnapFaceCore-release.aar
3. AirsnapFaceUI-release.aar




## step 1 . Import them into your  project in the project directory.



Include in build.gradle (module : app)

        implementation(files("libs/FingerprintModule.aar"))
        implementation(files("libs/opencv_411-release.aar"))
        implementation(files("libs/CompulynxAirSnap.aar"))
        implementation(files("libs/FaceCaptureModule.aar"))
        implementation (files("libs/AirsnapFaceCore-release.aar"))
        implementation (files("libs/AirsnapFaceUI-release.aar"))

        val camerax_version = "1.2.3"
        implementation( "androidx.camera:camera-core:${camerax_version}")
        implementation ("androidx.camera:camera-camera2:${camerax_version}")
        implementation ("androidx.camera:camera-lifecycle:${camerax_version}")
        implementation ("androidx.camera:camera-view:${camerax_version}")


In the the android block in app build.gradle

            packaging {
                jniLibs.pickFirsts.add("lib/x86/libc++_shared.so")
                jniLibs.pickFirsts.add("lib/x86_64/libc++_shared.so")
                jniLibs.pickFirsts.add("lib/armeabi-v7a/libc++_shared.so")
                jniLibs.pickFirsts.add("lib/arm64-v8a/libc++_shared.so")
            }

#   Use FingerPrint capture Sdk
Initialise the FingerCaptureActivity class

        val mFingerCaptureActivity = FingerCaptureActivity()
        mFingerCaptureActivity.setPositionCodeIndex(0);
        mFingerCaptureActivity.setUsername(userName);
        mFingerCaptureActivity.setLivenessCheck(liveNessCheck);
        mFingerCaptureActivity.setShowEllipses(ellipsesChecked);
        mFingerCaptureActivity.setCreateTemplates(createTemplates);
        mFingerCaptureActivity.setOrientationCheck(orientationCheck);
        mFingerCaptureActivity.setSaveSdkLogFlag(false);
        mFingerCaptureActivity.setDetectorThreshold(threshold);

## Navigate to the FingerCaptureActivity for scanning
        val i = Intent(this, mFingerCaptureActivity::class.java)
        resultLauncher.launch(i)

##
        private var resultLauncher =
         registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
            if (result.resultCode == RESULT_OK) {

                val receivedFingerprintList: ArrayList<FingerPrint>? =
                    result.data?.getParcelableArrayListExtra("list")


                if (!receivedFingerprintList.isNullOrEmpty()) {
                    binding.recyclerView.layoutManager = LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL, false)

                    binding.recyclerView.adapter  = FingerprintAdapter(this, receivedFingerprintList)
                }

            } else {

            }

        }

##
    val receivedFingerprintList: ArrayList<FingerPrint>? =
                result.data?.getParcelableArrayListExtra("list")

    //The receivedFingerprintList contain scanned Fingerprints 


##

# Use face capture sdk

create an instance of FaceCaptureActivity adding the proper settings variables.

     val faceCaptureActivity = FaceCaptureActivity.builder()
                                .setUseBackCamera(useBackCamera)
                                            .setUseAutoCapture(useAutoCapture)
                                            .setUseVideoCapture(useVideoCapture)
                                            .build()
    


     faceCaptureActivity.faceExtraction(this,this)

#
        .faceExtraction(context:Context,callbak FaceCaptureListener)


# FaceCaptureListener

    public interface FaceCaptureListener {
        void onFaceCaptured(byte[] var1, FaceBox var2);

        void OnFaceCaptureFailed(String var1);

        void onCancelled();

        void onTimedout(byte[] var1);
    }

#
    void onFaceCaptured(byte[] var1, FaceBox var2);

         .byte[] var1  - image bytes
         .FaceBox var2

#
    void OnFaceCaptureFailed(String var1);

         .String var1  - Error message

# 
    void onCancelled();

        . called when the task is cancelled

# 
       void onTimedout(byte[] var1);
