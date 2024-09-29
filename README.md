# mdgen_test
bjing2016/mdgen

## Installation
<pre>
pip install numpy==1.21.2 pandas==1.5.3
pip install torch==1.12.1+cu113 -f https://download.pytorch.org/whl/torch_stable.html
pip install pytorch_lightning==2.0.4 mdtraj==1.9.9 biopython==1.79
pip install wandb dm-tree einops torchdiffeq fair-esm pyEMMA
pip install matplotlib==3.7.2 numpy==1.21.2
</pre>

## Datasets

1. Download the tetrapeptide MD datasets:
<pre>
mkdir -p data/4AA_sims data/4AA_sims_implicit
gsutil rsync -r gs://mdgen/4AA_sims data/4AA_sims
gsutil rsync -r gs://mdgen/4AA_sims_implicit data/4AA_sims_implicit
</pre>
2. Download the ATLAS simulations via
 https://github.com/bjing2016/alphaflow/blob/master/scripts/download_atlas.sh to data/atlas_sims.
3. Preprocess the tetrapeptide simulations
# Forward simulation and TPS, prep with interval 100 * 100fs = 10ps
<pre>
python -m scripts.prep_sims --splits splits/4AA.csv --sim_dir data/4AA_sims --outdir data/4AA_data --num_workers [N] --suffix _i100 --stride 100
</pre>
# Upsampling, prep with interval 100fs
<pre>
python -m scripts.prep_sims --splits splits/4AA_implicit.csv --sim_dir data/4AA_sims_implicit --outdir data/4AA_data_implicit --num_workers [N]
</pre>
# Inpainting, prep with interval 100fs 
<pre>
python -m scripts.prep_sims --splits splits/4AA.csv --sim_dir data/4AA_sims --outdir data/4AA_data --num_workers [N]
</pre>
