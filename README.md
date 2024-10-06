# mdgen_test
bjing2016/mdgen

<pre>
  MDGen

Implementation of Generative Modeling of Molecular Dynamics Trajectories by Bowen Jing*, Hannes Stark*, Tommi Jaakkola, and Bonnie Berger.

We introduce generative modeling of molecular trajectories as a paradigm for learning flexible multi-task surrogate models of MD from data. By conditioning on appropriately chosen frames of the trajectory, such generative models can be adapted to diverse tasks such as forward simulation, transition path sampling, and trajectory upsampling. By alternatively conditioning on part of the molecular system and inpainting the rest, we also demonstrate the first steps towards dynamics-conditioned molecular design. We validate these capabilities on tetrapeptide simulations and show initial steps towards learning trajectories of protein monomers. Methodological details and further evaluations can be found in the paper. Please feel free to reach out to us at bjing@mit.edu, hstark@mit.edu with any questions.

</pre>pre>

## Installation
<pre>
pip install numpy==1.21.2 pandas==1.5.3
pip install torch==1.12.1+cu113 -f https://download.pytorch.org/whl/torch_stable.html
pip install pytorch_lightning==2.0.4 mdtraj==1.9.9 biopython==1.79
pip install wandb dm-tree einops torchdiffeq fair-esm pyEMMA
pip install matplotlib==3.7.2 numpy==1.21.2
</pre>

## Datasets

0. prepare

  Here is how you can do it in Ubuntu:
 <pre>

sudo apt-get install apt-transport-https ca-certificates gnupg
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add - 
sudo apt-get update && sudo apt-get install google-cloud-cli
 </pre>

2. Download the tetrapeptide MD datasets:
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

## References

bjing2016/mdgen

https://github.com/bjing2016/mdgen

linux - How to configure gsutil? - Stack Overflow

https://stackoverflow.com/questions/7989741/how-to-configure-gsutil

