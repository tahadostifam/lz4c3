# LZ4 binding for C3 Language

### Dependencies
C3 Compiler Version: 0.6.7 

```bash
sudo zypper in liblz4-1 liblz4-devel 
```

### Unit Tests
```
c3c test -l lz4
```

### Examples
import std::io;
import libc;
import lz4;

```c3
fn void main() {
    ZString input = "This is a test string to compress using LZ4!";
    int originalSize = (int) libc::strlen(input) + 1; 

    int maxCompressedSize = lz4::compress_bound(originalSize); 

    char* compressedData = (char*) malloc(maxCompressedSize);
    defer free(compressedData);
    if (compressedData == null)
    {
        abort("memalloc failed");
    }

    int compressedSize = lz4::compress_default(input, compressedData, originalSize, maxCompressedSize);
    if (compressedSize <= 0)
    {	
        abort("compression failed");
    }

    char *decompressedData = (char *)malloc(originalSize);  
    defer free(decompressedData);
    if (!decompressedData) {
        abort("memalloc failed");
    }

    int decompressedSize = lz4::decompress_safe(compressedData, decompressedData, compressedSize, originalSize);

    assert(decompressedSize == originalSize);
}
```

### Contribution

Feel free to create issue/PR if you encountered any problem.