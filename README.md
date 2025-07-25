# Single Cell Data Export Pipeline

## Overview
This R script exports Seurat single-cell RNA-seq data to tab-separated format for downstream analysis. It processes large datasets using chunked memory-efficient operations.

## Output Files
- **Dense matrix**: `*_gene_by_cell_matrix.tsv` - Log-normalized expression values (genes × cells)
- **UMAP coordinates**: `umap-*.tsv` - 2D embedding coordinates  
- **Cell metadata**: `metadata-*.tsv` - Cell annotations and QC metrics

## Resource Requirements

### Memory Allocation Guidelines
| Dataset Size | Memory | CPUs | Time Estimate |
|-------------|--------|------|---------------|
| ~300K cells | 800GB | 12 | ~15 minutes |
| ~150K cells | 400GB | 8 | ~8 minutes |
| ~75K cells | 200GB | 6 | ~4 minutes |
| ~30K cells | 100GB | 4 | ~2 minutes |

### Galyleo Launch Command Template
```bash
galyleo launch -p platinum -q hcp-csd854 -A csd854 \
  --time-limit 9:00:00 \
  --conda-env r44_env_v2 \
  --memory [MEMORY_GB] \
  --cpus [CPU_COUNT]
```

### Example for Different Dataset Sizes
```bash
# Large dataset (300K+ cells)
galyleo launch -p platinum -q hcp-csd854 -A csd854 --time-limit 9:00:00 --conda-env r44_env_v2 --memory 800 --cpus 12

# Medium dataset (100-300K cells)
galyleo launch -p platinum -q hcp-csd854 -A csd854 --time-limit 6:00:00 --conda-env r44_env_v2 --memory 400 --cpus 8

# Small dataset (<100K cells)
galyleo launch -p platinum -q hcp-csd854 -A csd854 --time-limit 3:00:00 --conda-env r44_env_v2 --memory 200 --cpus 6
```

## Key Features
- **Chunked processing**: Processes 500 genes at a time to manage memory
- **Progress tracking**: Real-time progress updates with time estimates
- **Memory optimization**: Immediate cleanup of processed chunks
- **Parallel processing**: Utilizes multiple CPUs with future package

## Usage Notes
- Adjust `chunk_size` parameter if memory issues persist
- Monitor peak memory usage stays under allocated limits
- Dense matrix files can be very large (>100GB for large datasets)
- Processing time scales roughly linearly with cell count
