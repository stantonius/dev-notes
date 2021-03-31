# Notes on DL training

To view the GPU stats using the Windows terminal: 
* `nvidia-smi dmon` - and look at the `sm` column
* `watch -n 0.5 nvidia-smi` - refresh the GPU stats every `0.5` seconds
* `watch -n 1 free -m` - watch the memory usage

## Kaggle setup

`!echo '{"username":"canadarmy","key":"xxxxxxINSERTKEYxxxxxx"}' > ~/.kaggle/kaggle.json`