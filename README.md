# Article_ML_VS_MG
Just a simple repo with config file to reproduce the results in our article

You have to install exastencils: https://github.com/lssfau/ExaStencils

  The first step is installing sbt -- a guide can be found here

  Next, open a shell and locate the folder the git repository has been checked out to. Compilation is done via typing
  
  sbt compile
  
  To assemble a jar the following command is available
  
  sbt assembly
  
  Now you should have an ExaStencils binary file

Then, you can reproduce the result by creating the cpp file from the files in this repo. java -cp $ExaStencils_Path Main $Settings_Path $Knowledge_Path

To reproduce all our points, you have to change the maxlevel in the knowledge file and optionally the output_path in the settings file, with value between 4 and 14.
