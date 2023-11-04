**Exploiting Directory Listing and Retrieving Leaked Information**

- If directory listing is enabled, you can browse through the files and retrieve leaked information.

**Using wget for Mass-Download with Directory Listing**
- The `wget` command retrieves content from web servers. You can use `wget` in recursive mode (-r) to mass-download all files stored within the specified directory and its subdirectories:

```bash
$ wget -r example.com/.git
```

**Reconstructing .git Directory Without Directory Listing**
- If directory listing isn't enabled, and the directory's files are not shown, you can still reconstruct the entire `.git` directory. First, confirm that the folder's contents are available to the public by trying to access the directory's config file:

```bash
$ curl https://example.com/.git/config
```

- If the config file is accessible, you might be able to download the `.git` directory's entire contents.

**Structure of .git Directory**
- A `.git` directory is laid out in a specific way, with important files and folders. For example:

```bash
$ ls .git
COMMIT_EDITMSG HEAD branches config description hooks index info logs objects refs
```

- The `/objects` directory is used to store Git objects, and within it, you'll find subdirectories and files named after the SHA1 hash of the Git objects stored in them.

**Determining Object Type**
- You can determine an object's type using the following command:

```bash
$ git cat-file -t OBJECT-HASH
```

- Different types of objects are stored in `.git/objects`, including commits, trees, blobs, and annotated tags.

**Displaying Git Object Content**
- You can display the content of a Git object using:

```bash
$ git cat-file -p OBJECT-HASH
```

**Accessing Git Configuration and HEAD**
- The `/config` file is the Git configuration file for the project, and the `/HEAD` file contains a reference to the current branch.

```bash
$ cat .git/HEAD
ref: refs/heads/master
```

**Finding Files Without Directory Listing**
- If you can't access the `.git` folder's directory listing, you have to download each file you want individually. Start with filepaths you already know exist, like `.git/HEAD`, and follow the references to discover more files.

**Remote Server Examples**
- On a remote server, use URLs to access the Git information:

- To determine the HEAD:
  ```
  https://example.com/.git/HEAD
  ```

- To find the object stored in that HEAD:
  ```
  https://example.com/.git/refs/heads/master
  ```

- To access the tree associated with the commit:
  ```
  https://example.com/.git/objects/0a/72e6850ef963c6aeee4121d38cf9de773865d8
  ```

- To download the source code stored in a specific file:
  ```
  https://example.com/.git/objects/4b/66088945aab8b967da07ddd8d3cf8c47a3f53c
  ```

**Decompressing Object Files**
- When downloading files from a remote server, you may need to decompress the downloaded object files before reading them using Ruby, Python, or your preferred language's zlib library.

- Ruby:
  ```bash
  ruby -rzlib -e 'print Zlib::Inflate.new.inflate(STDIN.read)' < OBJECT_FILE
  ```

- Python:
  ```bash
  python -c 'import zlib, sys; print repr(zlib.decompress(sys.stdin.read()))' < OBJECT_FILE
  ```

- Credits to Vickie Li.
