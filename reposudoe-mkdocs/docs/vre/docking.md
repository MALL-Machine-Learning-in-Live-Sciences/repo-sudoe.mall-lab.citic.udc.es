---
comments: true
---

# Molecular Docking with Autodock-Vina-docker

## Docker Image Import

First, we will download the custom Docker image from the following link **TBD** and store it in the `data` folder. Afterwards, we will load the image into our system and check that everything is in order.

```shell
docker load -i mall-lab-docking-vina-all-20241012A.tar.gz
docker image ls
```

## Autodock-Vina Pipeline

Once we have the image ready, we can perform the docking. This is a basic tutorial extracted from the [Autodock-Vina documentation](https://autodock-vina.readthedocs.io/en/latest/docking_basic.html). Below is a brief explanation of the inputs, outputs, and steps to follow.

### Inputs (both can be read in a text editor):

- Receptor in `pdb` format (or similar).
- Ligand in `sdf` format (or similar).

### Intermediate Files

Intermediate files will be generated in steps 1 through 4:

1. Receptor in `pdbqt` format
2. Ligand in `pdbqt` format
3. Coordinates in `gpf` and `glg` formats for the receptor
4. `map` files for creating interaction zones.

### Outputs:

- We save a `log` file with the output from Vina.
- We save the `pdbqt` file with the changes in the ligand.
- We save the `pdbqt` receptor for plotting.

### Basic Molecular Docking Steps:

#### 1. Prepare Receptor

```shell
docker run -v $PWD:/data --rm mall-lab/docking-vina-all:v1.2.5 \
prepare_receptor -r $receptor_pdb -o $receptor_pdbqt
```

#### 2. Prepare Ligand
```shell
docker run -it -v $PWD:/data --rm mall-lab/docking-vina-all:v1.2.5 \
bash -c "source /root/.bashrc && mk_prepare_ligand.py -i $ligando_sdf -o $ligando_pdbqt"
```

#### 3. Map the Interaction Zone Between Receptor and Ligand
```shell
docker run -it -v $PWD:/data --rm mall-lab/docking-vina-all:v1.2.5 \
bash -c "source /root/.bashrc && pythonsh /../opt/AutoDock-Vina/example/autodock_scripts/prepare_gpf.py \
-l $ligando_pdbqt -r $receptor_pdbqt -o $gpf_file -y"
```

#### 4. Generate Interaction Map
```shell
docker run -v $PWD:/data --rm mall-lab/docking-vina-all:v1.2.5 \
autogrid4 -p $gpf_file -l $glg_file
```

#### Run Vina
```shell
docker run -v $PWD:/data --rm mall-lab/docking-vina-all:v1.2.5 \
vina --ligand $ligando_pdbqt --maps $map_file --scoring ad4 \
--exhaustiveness 32 --out $output_pdbqt \
--cpu 30 --seed -556654859 --verbosity 2 > $log_file 2>&1
```

## Additional Folder Information

In the `data` folder we have:

- ligands: folder with ligands (drugs) in `sdf` format.
- receptors: folder with receptors (protein) in `pdb` format.
- mall-lab-docking-vina-all: the custom Autodock-Vina image.

In the `results` folder we will have the results sorted by name (ligand-receptor pair).

In the `scripts` folder we have:

- The `00_basic_docking.sh` script: which contains all the steps to perform the docking and then clean up the workspace.
- The `config.ini` file: config file where we parse the input files.
