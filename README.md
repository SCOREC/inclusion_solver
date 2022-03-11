## Summary

This application (based on MFEM's Volta miniapp), solve the electrostatic problem for a substrate the includes an array of ellipsoidal inclusions with a different permittivity. The boundary conditions for this problem are consistent with a uniform background Electric field in the z direction of the coordinate axes.

## Build Instructions

* Edit config.sh and provide installation path for `MFEM_INSTALL_PATH`(location of the MFEM installation), `HYPRE_DIR` (location of Hypre installation) and `METIS_DIR` (location of Metis installation)
* mkdir build
* cd build
* source ../config.sh
* make

If things run successfully there should be an executable named `inclusion` in your build folder.

**Note** for SCOREC users. All the necessary module to build can be loaded by calling `source ../env-setup-scorec.sh`, and `scorec_config.sh` config file is customized for SCOREC.

**Note** both the build for this example and the build of MFEM have to used consistent dependencies (e.g. the same Hypre, Metis, etc) to avoid build problems.

## Example Meshes

You can find two example meshes in the folder `./meshes`. The first one `setup_1x1_global_coarse_template_mesh.mesh` has only 1 ellipsoidal inclusion in the substrate. The second one `setup_5x5_global_coarse_template_mesh.mesh` has a 5by5 array of identical ellipsoidal inclusions in the substrate. Both of these meshes have appropriate classification information.

## Running Cases

You can use the executable `inclusion` as follows:

```
mpirun -np nnn ./inclusion --mesh path_to_mesh --substrate 'substrate-model-tag-list' --inclusion 'inclusion-model-tag-list' -dbcs 'drichlet-bc-model-tag-list'
```

* `substrate-model-tag-list`: space separated list of integers (tags) for the model regions making up the phase 1 or substrate
* `inclusion-model-tag-list`: space separated list of integers (tags) for the model regions making up the phase 2 or inclusions
* `drichlet-bc-model-tag-list`: space separated list of integers (tags) for the model faces that needs to be included in the Dirichlet bc.


Specifically for the two meshes provided with this example the command to run looks like

```
mpirun -np 4 ./inclusion --mesh ../meshes/setup_1x1_global_coarse_template_mesh.mesh --substrate '92' --inclusion '186' -dbcs '24 42 76 78 80 82'
```

```
mpirun -np 4 ./inclusion --mesh ../meshes/setup_5x5_global_coarse_template_mesh.mesh --substrate '92' --inclusion '2590 2435 4321 2282 714 2757 875 2918 3861 1823 4494 3711 4184 2145 4031 1992 1366 3409 1527 3570 1060 3103 1221 3264 1696' -dbcs '24 42 76 78 80 82'
```