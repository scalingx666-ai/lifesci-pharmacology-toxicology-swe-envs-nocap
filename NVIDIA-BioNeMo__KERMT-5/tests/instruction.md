# Issue

**Title:** Disallow inconsistent usage of finetune and predict workflows

## Description

This PR fixes inconsistent usage of finetune and predict workflows. 

### First inconsistent usage pattern
1. Finetune with `--rdkit2D_normalization_type descriptastorus`
`python main.py finetune \
     --data_path tests/data/finetune/train.csv \
     --separate_val_path tests/data/finetune/val.csv \
     --separate_test_path tests/data/finetune/test.csv \
     --save_dir test_run/finetune \
     --checkpoint_path ~/path/to/pretrained.pt \
     --dataset_type regression --split_type scaffold_balanced \
     --ensemble_size 1 --num_folds 1 --no_features_scaling \
     --ffn_hidden_size 700 --ffn_num_layers 3 --bond_drop_rate 0.1 \
     --epochs 4 --metric mae --self_attention --dist_coff 0.15 \
     --max_lr 1e-4 --final_lr 2e-5 --dropout 0.0 \
     --use_cuikmolmaker_featurization \
     --features_generator rdkit_2d_normalized_cuik_molmaker \
     --rdkit2D_normalization_type descriptastorus --seed 50
 `
2. Incorrectly predict with ` --rdkit2D_normalization_type fast`
`python main.py predict      --data_path tests/data/finetune/test.csv      --checkpoint_dir test_run/finetune/      --no_features_scaling      --features_generator rdkit_2d_normalized_cuik_molmaker      --rdkit2D_normalization_type fast      --output test_run/predict_inconsistent/predict.csv`
This usage is disallowed:
`ValueError: Argument rdkit2D_normalization_type is not consistent between finetune and predict. Finetune value: fast, Predict value: descriptastorus`

### Second inconsistent usage pattern
1. Finetune with `--features_generator rdkit_2d_normalized_cuik_molmaker`
`python main.py finetune \
     --data_path tests/data/finetune/train.csv \
     --separate_val_path tests/data/finetune/val.csv \
     --separate_test_path tests/data/finetune/test.csv \
     --save_dir test_run/finetune \
     --checkpoint_path ~/path/to/pretrained.pt \
     --dataset_type regression --split_type scaffold_balanced \
     --ensemble_size 1 --num_folds 1 --no_features_scaling \
     --ffn_hidden_size 700 --ffn_num_layers 3 --bond_drop_rate 0.1 \
     --epochs 4 --metric mae --self_attention --dist_coff 0.15 \
     --max_lr 1e-4 --final_lr 2e-5 --dropout 0.0 \
     --use_cuikmolmaker_featurization \
     --features_generator rdkit_2d_normalized_cuik_molmaker \
     --rdkit2D_normalization_type descriptastorus --seed 50`
2. Predict without `--features_generator rdkit_2d_normalized_cuik_molmaker`
`python main.py predict      --data_path tests/data/finetune/test.csv      --checkpoint_dir test_run/finetune/      --no_features_scaling      --rdkit2D_normalization_type descriptastorus      --output test_run/predict_inconsistent/predict.csv`
Disallows inconsistent usage
`    raise ValueError(f'Argument {key} is not consistent between finetune and predict. '
ValueError: Argument features_generator is not consistent between finetune and predict. Finetune value: None, Predict value: ['rdkit_2d_normalized_cuik_molmaker']`

### Pin `setuptools`
Pin `setuptools<82.0.0` for `pkg_resources`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `918c5c144763`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.