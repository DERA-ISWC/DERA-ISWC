# DERA: Dense Entity Retrieval for Entity Alignment

DERA is a dense entity retrieval framework for EA, leveraging language models to uniformly encode various features of entities and facilitate nearest entity search across KGs. Candidate alignments are first generated through entity retrieval, which are subsequently reranked to determine the final alignments.

------

## Installation

Ensure your Python version is 3.6 or higher. It is recommended to install dependencies listed in the `requirements.txt` file:

```bash
https://github.com/DERA-ISWC/DERA-ISWC.git
cd aligncraft
pip install -r requirements.txt
pip install -e .
```

------

## Environment & Hardware

- This project was developed and tested with the following environment and hardware specifications:
  - **Python Version**: 3.6 or higher
  - **GPU (Recommended for Training)**:
    - **Entity Verbalization**: 8 × NVIDIA A800 GPUs
    - **Entity Retrieval & Reranking**: 2 × NVIDIA A800 GPUs
  - **Training Optimizations**:
    - DeepSpeed (Zero Stage 3)
    - Gradient Checkpointing
    - Flash Attention v2

------

## Project Preparation

You can download all the required **datasets** from the following link:

📂 **[Download Datasets](https://drive.google.com/file/d/1UEWVpLEnnnVmf6tofsNS1GwmtjCflKp1/view?usp=sharing)**

After downloading, extract the contents and place them into the `benchmark` directory. Make sure the paths in the configuration files point to the correct dataset locations before running the pipeline.

------

## Quick Start

DERA is structured into a three-stage pipeline:

1. **Entity Verbalization**: Transforms knowledge graph triples into natural language entity descriptions.
2. **Entity Retrieval**: Encodes textual descriptions into embeddings and retrieves top-k similar entities.
3. **Alignment Reranking**: Refines the alignment results through an interaction-based reranking model.

All full pipeline scripts are located in:

```
aligncraft/examples/retrievalea/pipeline/
```

To run the complete DERA pipeline on the `DBP15K fr-en` dataset:

```bash
bash aligncraft/examples/retrievalea/pipeline/attr/pipeline_fr_en.sh
```

### ⚙️ Customizing Execution

You may edit the `pipeline_fr_en.sh` script to run specific stages only. For instance, due to the time-consuming nature of entity verbalization (which generates training data from the KG), it is recommended to **run different stages separately if available**. To do so, you can comment out the unused stages in the shell script and execute them sequentially or in parallel on different devices.

------

## Output Directory Structure

- `~/.cache/aligncraft/`: Stores intermediate text descriptions (HDF5 format, keyed by MD5)
- `aligncraft/examples/retrievalea/logs/`: Logs for each pipeline step
- `aligncraft/examples/retrievalea/config/`: YAML configuration files for each language pair

------

## Key Scripts

- `generate_seq.py`: Converts KG triples into text descriptions
- `retrieval_test.py`: Evaluates untrained retrieval models
- `generate_retrieval_sft_data.py`: Generates supervised training data for retrieval
- `hn_mine_retrieval_sft_data.py`: Performs hard negative mining for retrieval training
- `retriever_finetune.py`: Fine-tunes the retrieval model
- `reranker_finetune.py`: Fine-tunes the reranker model
- `rerank_test.py`: Evaluates reranking performance under different configurations