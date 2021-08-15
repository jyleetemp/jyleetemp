---
title: "How to use LMDB with PyTorch `DataLoader` and `DistributedDataParallel`"
date: 2021-03-09 19:45:13 +0900
tags: [PyTorch]
toc: false
categories: [Research, PyTorch]
---
Since LMDB cannot be pickled, an error, `...can't pickle Environment Object...`, occurs when we naively implement LMDB into `data.dataset` while wrapping `data.DataLoader` with `Distributed Data Parallel`(`DDP`).
To resolve the error, we need to delay the loading of LMDB environment in `data.dataset`;

```python
# This code is modified from https://raw.githubusercontent.com/rmccorm4/PyTorch-LMDB/master

class my_dataset_LMDB(data.Dataset):
    def __init__(self, db_path, file_path) 
        self.db_path = db_path
        self.file_path = file_path

        # Delay loading LMDB data until after initialization to avoid "can't pickle Environment Object error"
        self.env = None
        self.txn = None

    def _init_db(self):
        self.env = lmdb.open(self.db_path, subdir=os.path.isdir(self.db_path),
            readonly=True, lock=False,
            readahead=False, meminit=False)
        self.txn = self.env.begin()

    def read_lmdb(self, key):
        lmdb_data = self.txn.get(key.encode())
        lmdb_data = np.frombuffer(lmdb_data)

        return lmdb_data

    def __getitem__(self, index):
        # Delay loading LMDB data until after initialization
        if self.env is None:
            self._init_db()

        file_name = self.file_paths[index]
        data = self.read_lmdb(file_name)
        ...
```

**Note:** By the way, the option `lock=False` fixes the error, `MDB_READERS_FULL: Environment maxreaders limit reached`.
{: .notice--info}


## References
[PyTorch-LMDB](https://github.com/rmccorm4/PyTorch-LMDB)

- - -
