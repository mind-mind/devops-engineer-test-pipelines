# DevOps Engineer Homework

## Task D

### How did you test your pipelines?

I ran Jenkins locally and tested Pipeline B and Pipeline C. I saved the Jenkins console logs in this branch as `taskB.log` and `taskC.log`.

### How did you test the Python parser in Repo C?

I ran:

```bash
python3 parser.py doxygen_warnings.log doxygen_report.csv
```

### What is the advantage of using Git LFS for binaries in Repo A?

Git LFS helps keep a repo small when it has large binary files. Git keeps a small reference to each file, while LFS stores the actual file. This can make cloning the repo faster and still lets us keep file versions.


### How can this repository be adjusted to support Git LFS?

use command

```bash
git lfs install
git lfs track "*.bin"
git add .gitattributes
git add path/to/large-file.bin
git commit -m "Track binary files with Git LFS"
git push
```
References:

- [Git LFS](https://git-lfs.com/)
- [GitHub: Configuring Git LFS](https://docs.github.com/en/repositories/working-with-files/managing-large-files/configuring-git-large-file-storage)


### Are there easier alternatives to Git LFS?

Yes. We can keep the binary files outside Git and save generated files as Jenkins artifacts. This is easier, but the binaries won’t be versioned with the source code.