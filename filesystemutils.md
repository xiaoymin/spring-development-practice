`FileSystemUtils`是文件系统操作工具类,提供清空目录、拷贝文件等方法

该类主要公开两个方法

* `public static boolean deleteRecursively(File root)`:清空root目录所有文件
* `public static void copyRecursively(File src, File dest)`:将源文件src内容拷贝到目标文件dest目录

以下是源码:

```java
package org.springframework.util;

import java.io.File;
import java.io.IOException;

/**
 * Utility methods for working with the file system.
 * 文件系统的一些操作方法
 * @author Rob Harrop
 * @author Juergen Hoeller
 * @since 2.5.3
 */
public abstract class FileSystemUtils {

     /**
     * 清空root目录所有文件
     * Delete the supplied {@link File} - for directories,
     * recursively delete any nested directories or files as well.
     * @param root the root {@code File} to delete
     * @return {@code true} if the {@code File} was deleted,
     * otherwise {@code false}
     */
    public static boolean deleteRecursively(File root) {
        //判断root目录是否为空,目录是否存在
        if (root != null && root.exists()) {
            //判断root是否是文件夹
            if (root.isDirectory()) {
                File[] children = root.listFiles();
                    if (children != null) {
                        for (File child : children) {
                            //递归清空目录
                            deleteRecursively(child);
                        }
                    }
                }
                return root.delete();
        }
        return false;
    }

    /**
     * 拷贝文件到目标文件
     * Recursively copy the contents of the {@code src} file/directory
     * to the {@code dest} file/directory.
     * @param src the source directory
     * @param dest the destination directory
     * @throws IOException in the case of I/O errors
     */
    public static void copyRecursively(File src, File dest) throws IOException {
        //源文件是不能为空，必须是目录或文件
        Assert.isTrue(src != null && (src.isDirectory() || src.isFile()), "Source File must denote a directory or file");
        //目标文件不能为空
        Assert.notNull(dest, "Destination File must not be null");
        doCopyRecursively(src, dest);
    }

    /**
     * 实际拷贝文件方法
     * Actually copy the contents of the {@code src} file/directory
     * to the {@code dest} file/directory.
     * @param src the source directory
     * @param dest the destination directory
     * @throws IOException in the case of I/O errors
     */
    private static void doCopyRecursively(File src, File dest) throws IOException {
        //源文件是目录,循环递归
        if (src.isDirectory()) {
            //创建目标文件夹目录
            dest.mkdir();
            File[] entries = src.listFiles();
            if (entries == null) {
                throw new IOException("Could not list files in directory: " + src);
            }
            for (File entry : entries) {
                doCopyRecursively(entry, new File(dest, entry.getName()));
            }
        }
        //src是文件
        else if (src.isFile()) {
            try {
                dest.createNewFile();
            }
            catch (IOException ex) {
                IOException ioex = new IOException("Failed to create file: " + dest);
                ioex.initCause(ex);
                throw ioex;
            }
            //执行copy
            FileCopyUtils.copy(src, dest);
        }
        else {
            // Special File handle: neither a file not a directory.
            // Simply skip it when contained in nested directory...
        }
    }


}
```



