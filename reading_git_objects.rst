###################
Reading git objects
###################

By default objects in ``.git/objects`` are very simple zlib compressed files.
These are very easy to read with a small amount of code.

git will do this for you with ``git show``.

When a repository gets larger, git may use another, less simple format to
store the data, called *packfiles*, in which case these simple code fragments
won't work.  They are only show the simplicity of the basic git model.

I'll generate a small git repository to make an object file.

.. runblock::
    :hide:

    rm -rf git_example

.. runblock::

    mkdir git_example
    cd git_example
    echo "My data fits on one line" > my_file.txt
    git init
    git add my_file.txt
    tree -a .git/objects

******
Python
******

.. testcode::

    import zlib # A compression / decompression library

    def read_git_object(filename):
        """ Read an object in the ``.git/objects`` directory
        """
        compressed_contents = open(filename, 'rb').read()
        return zlib.decompress(compressed_contents)

    filename = 'git_example/.git/objects/e2/7bb34b0807ebf1b91bb66a4c147430cde4f08f'
    new_object_data = read_git_object(filename)
    print(new_object_data)

.. testoutput::
    :options: +NORMALIZE_WHITESPACE

    blob 25 My data fits on one line

The contents of the object file is the original contents prepended by
``blob25`` and a 0 byte, where 25 is the number of characters in the file.
