# booky

This script creates bookmarks of a pdf from a simple text file. The tool `pdftk` can already do this in fact
internally I am using that tool itself. But `pdftk` requires a format which is too tedious to write. So I have written
this script to enter bookmarks data in a simple format.

## Dependencies

* `bash`
* `python3`
* `pdftk`
* `dirname`
* `basename`
* `GNU sed` (OSX users take note, you may have BSD sed. Install `gsed` instead)

## Bookmark format

* Every level starts with a `{` on a _separate line_.
* Bookmarks have title with page number separated by comma.
* Both title and page number should be on the same line.
* All these are equivalent (i.e. the script is whitespace agnostic).

  ```plain
  title1, 1
  title1,             1
        title1     ,         1
  ```

### Example

  ```plain
  {
    Title1, 1
    Title2, 2
    {
      Subtitle1, 3
      Subtitle2, 4
      {
        SubSubtitle1, 5
        ...
      }
    }
  }
  ```

## How To Use it?

* First clone this repository and change your directory. Execute this in a terminal

```shell
git clone https://github.com/SiddharthPant/booky.git
cd booky
```

* Now copy your pdf file to this directory
* Create a new text file and write your bookmarks in the given format
* Now your directory should contain 4 files: `booky.sh`, `booky.py`, `your_pdf_file.pdf`, `your_text_file.txt`
* Write the following commands in the terminal

```shell
./booky.sh your_pdf_file.pdf your_text_file.txt
```

If you add the `booky` directory to the environment PATH like:

```shell
export PATH=/path_to_the_booky:$PATH
```

then it can run from any directory:

```shell
booky.sh your_pdf_file.pdf your_text_file.txt
```

This creates a new pdf file `your_pdf_file_new.pdf` with your bookmarks.

This is going to work in POSIX systems, but if instead you are on a Windows machine. Then first install `python3` and `pdftk` just use the `booky.py` file in the repo to convert `bkmrks.txt` to `pdftk` compatible format

```shell
python3 booky.py < bkmrks.txt > output.txt
```

use the export command to generate a dumped data file.

```shell
pdftk C:\Users\Sid\Desktop\doc.pdf dump_data output C:\Users\Sid\Desktop\doc_data.txt
```

Remove the previous bookmarks from that file and insert content of `output.txt` instead using a simple copy & paste.
And then import that data back.

```shell
pdftk C:\Users\Sid\Desktop\doc.pdf update_info C:\Users\Sid\Desktop\doc_data.txt output C:\Users\Sid\Desktop\updated.pdf
```

If this does not update your bookmarks check that your `pdftk` version is greater than 1.45.
