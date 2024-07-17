# Article_ML_VS_MG
Just a simple repo with config file to reproduce the results in our article

You have to install exastencils: https://github.com/lssfau/ExaStencils

  The first step is installing sbt -- a guide can be found here: https://www.scala-sbt.org/1.x/docs/Installing-sbt-on-Linux.html
  
  Next, open a shell and locate the folder the git repository has been checked out to. Compilation is done via typing
  
  sbt compile
  
  To assemble a jar the following command is available
  
  sbt assembly
  
  Now you should have an ExaStencils binary file

Then, you can reproduce the result by creating the cpp file from the exa4, .settings and .knowledge files in this repo. Just copy them in a folder (ex: 2D_FD_Poisson_from_L4) inside the Examples directrectory, then cd in Examples dir and create the cpp file by doing: java -cp $ExaStencils_Path Main $Settings_Path $Knowledge_Path lib/linux.platform (compiler path should be ../Compiler/Compiler.jar, $Settings_Path should be 2D_FD_Poisson_from_L4/2D_FD_Poisson_from_L4.settings and $Knowledge_Path should be 2D_FD_Poisson_from_L4/2D_FD_Poisson_from_L4.knowledge)

To reproduce all our points, you have to change the maxlevel in the knowledge file and optionally the output_path in the settings file, with value between 4 and 14. Here is an example with python:

```python
for depth in [4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]:
        file_path = "./2D_FD_Poisson_from_L4/2D_FD_Poisson_from_L4.knowledge"
        with open(file_path, 'r') as file:
                lines = file.readlines()
        with open(file_path, 'w') as file:
                for i, line in enumerate(lines):
                        if "maxLevel" in line:
                                file.write("maxLevel                       = " + str(depth - 1) + "\n")
                        else:
                                file.write(line)
        file_path = ".2/D_FD_Poisson_from_L4/2D_FD_Poisson_from_L4.settings"
        with open(file_path, 'r') as file:
                lines = file.readlines()
        with open(file_path, 'w') as file:
                for i, line in enumerate(lines):
                        if "outputPath" in line:  # Python uses 0-based indexing
                                file.write('outputPath          = "../generated/depth_' + str(depth) + '/$configName$' + '/"\n')
                        else:
                                file.write(line)
        os.system("java -cp ../Compiler/Compiler.jar Main 2D_FD_Poisson_from_L4/2D_FD_Poisson_from_L4.settings 2D_FD_Poisson_from_L4/2D_FD_Poisson_from_L4.knowledge lib/linux.platform")
```

Then compile and run the generated code on GPU be careful with the arch flag in cuda flag, it could be incompatible with your actual GPU

You have also to install Plasmanet: https://github.com/lionelchg/PlasmaNet.git
Careful some hard coded path should create issue when trying to replicate results, try grep -r /scratch/cfd* and change the line that appears with your actual paths
