# NanoSUSY-tools
Postprocessing script for Stop 0L analysis

### Set up CMSSW

```tcsh
source /cvmfs/cms.cern.ch/cmsset_default.csh
setenv SCRAM_ARCH slc6_amd64_gcc700
cmsrel CMSSW_10_2_6
cd CMSSW_10_2_6/src/
cmsenv
```

### Set up CMSSW
```tcsh
cd $CMSSW_BASE/src
cmsenv
git clone -b Stop0l git@github.com:susy2015/nanoAOD-tools.git PhysicsTools/NanoAODTools
git clone git@github.com:susy2015/NanoSUSY-tools.git PhysicsTools/NanoSUSYTools
scram b
```

### To run basic postprocessing:
To submit a job to condor, run the following:
```cd $CMSSW_BASE/src/PhysicsTools/NanoSUSYTools/python/processors/Condor
python SubmitLPC.py -c <config_file_era.cfg>
```
It is required to have the era (year of production) somewhere in the config file name.
This defaults to creating directories similar to the input filelist locations in the config file (replace "Pre" with "Post" and append a version number to the final directory name), with subdirectories named by the first entry of each line in the config file.
To specify a different location, append this to the above command:
```-o </path/to/location/>```
To test locally, run:
```cd $CMSSW_BASE/src/PhysicsTools/NanoSUSYTools/python/processors/Condor
python Stop0l_postproc.py -i <inputfilelist> -e <era (year)> --isFastSim <True/False> --isData <True/False> --crossSection <#, 1 for data> --nEvents <#>
```
The input filelist is a text file like those that are listed inside the config files.
The output location defaults to the same directory. To specify a different output location, use -o .
The other arguments listed are those required by the default postprocessing modules. If you are using a different set of modules which do not require these arguments, you do not need to include these arguments.


## To Do:
* PDF uncertainty module 
    * weights stored in NanoAOD accordingly already, need code to extract the envelope
* lepton SF module follow [SUSY](https://twiki.cern.ch/twiki/bin/viewauth/CMS/SUSLeptonSF#Scale_Factors_for_SUSY_IDs)
    * Need to update based upon the existing example code from NanoAOD-tools
* PU reweighting 
    * Example code existed, but need recompute the pileup distribution from data
* btag SF update (instead of the btag weight stored during production)
    * Need follow up with which method to be apply (iterative fit for DeepCSV?)
* JEC uncertainty update
    * Need to understand the existing tool in NanoAOD-Tools
* DeepAK8/DeepResolved SF
    * Are they available yet?
* update Jet ID : [JetID Twiki](https://twiki.cern.ch/twiki/bin/viewauth/CMS/JetID13TeVRun2018)
    * Do we need it in post-processing, or wait for next production?
* L1EcalPrefiring [twiki]* (https://twiki.cern.ch/twiki/bin/viewauth/CMS/L1ECALPrefiringWeightRecipe#Call_the_producer_in_your_config)
    * Rumor is it will be included in next NanoAOD
    * If not, we will make a module for it
* Various systematics 
* Trigger path and efficiency:
    * Once Hui's study is finalized, we will store the bit and efficiency + systematic
