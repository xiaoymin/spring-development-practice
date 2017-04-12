`FileCopyUtils`是文件系统操作工具类,提供拷贝文件等方法该类主要公开以下方法 ：

* `public static int copy(File in, File out)`:将给定的输入文件的内容复制到给定的输出文件中
* `public static void copy(byte[] in, File out)`:将给定的字节数组的内容复制到给定的输出文件
* `public static byte[] copyToByteArray(File in)`:将给定的输入文件的内容复制到新的字节数组中
* `public static int copy(InputStream in, OutputStream out)`:复制给定的InputStream的内容到给定的输出流。
* `public static void copy(byte[] in, OutputStream out)`:复制的字节数组给定的OutputStream的内容。
* `public static byte[] copyToByteArray(InputStream in)`:复制到一个新的给定的InputStream字节数组的内容
* `public static int copy(Reader in, Writer out)`:从reader对象拷贝到writer
* `public static void copy(String in, Writer out)`:将给定字符串的内容复制到给定的输出写入器
* `public static String copyToString(Reader in)`:将给定reader的内容复制到字符串中。

以下是源码:

```java
/*
 * Copyright 2002-2013 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package org.springframework.util;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.Reader;
import java.io.StringWriter;
import java.io.Writer;

/**
 * 文件和流复制的简单方法,所有的拷贝方法使用的字节数组大小为4096字节,并且当执行完毕后,关闭所有的流,
 * 流的一些操作方法可以参考StreamUtils.java
 *
 * Simple utility methods for file and stream copying. All copy methods use a block size
 * of 4096 bytes, and close all affected streams when done. A variation of the copy
 * methods from this class that leave streams open can be found in {@link StreamUtils}.
 *
 * <p>Mainly for use within the framework, but also useful for application code.
 *
 * @author Juergen Hoeller
 * @since 06.10.2003
 * @see StreamUtils
 */
public abstract class FileCopyUtils {

    public static final int BUFFER_SIZE = StreamUtils.BUFFER_SIZE;


    //---------------------------------------------------------------------
    // Copy methods for java.io.File
    //---------------------------------------------------------------------

    /**
     * 将给定的输入文件的内容复制到给定的输出文件中
     * Copy the contents of the given input File to the given output File.
     * @param in the file to copy from
     * @param out the file to copy to
     * @return the number of bytes copied 返回copy的字节大小
     * @throws IOException in case of I/O errors
     */
    public static int copy(File in, File out) throws IOException {
        Assert.notNull(in, "No input File specified");
        Assert.notNull(out, "No output File specified");
        return copy(new BufferedInputStream(new FileInputStream(in)),
            new BufferedOutputStream(new FileOutputStream(out)));
    }

    /**
     * 将给定的字节数组的内容复制到给定的输出文件
     * Copy the contents of the given byte array to the given output File.
     * @param in the byte array to copy from
     * @param out the file to copy to
     * @throws IOException in case of I/O errors
     */
    public static void copy(byte[] in, File out) throws IOException {
        Assert.notNull(in, "No input byte array specified");
        Assert.notNull(out, "No output File specified");
        //创建输入字节流
        ByteArrayInputStream inStream = new ByteArrayInputStream(in);
        //创建文件输出流
        OutputStream outStream = new BufferedOutputStream(new FileOutputStream(out));
        copy(inStream, outStream);
    }

    /**
     * 将给定的输入文件的内容复制到新的字节数组中
     * Copy the contents of the given input File into a new byte array.
     * @param in the file to copy from
     * @return the new byte array that has been copied to
     * @throws IOException in case of I/O errors
     */
    public static byte[] copyToByteArray(File in) throws IOException {
        Assert.notNull(in, "No input File specified");
        return copyToByteArray(new BufferedInputStream(new FileInputStream(in)));
    }


    //---------------------------------------------------------------------
    // Copy methods for java.io.InputStream / java.io.OutputStream
    //---------------------------------------------------------------------

    /**
     * 复制给定的InputStream的内容到给定的输出流。
     * 完成时关闭两个流。
     * Copy the contents of the given InputStream to the given OutputStream.
     * Closes both streams when done.
     * @param in the stream to copy from
     * @param out the stream to copy to
     * @return the number of bytes copied
     * @throws IOException in case of I/O errors
     */
    public static int copy(InputStream in, OutputStream out) throws IOException {
        Assert.notNull(in, "No InputStream specified");
        Assert.notNull(out, "No OutputStream specified");
        try {
            return StreamUtils.copy(in, out);
        }
        finally {
            try {
                in.close();
            }
            catch (IOException ex) {
            }
            try {
                out.close();
            }
            catch (IOException ex) {
            }
        }
    }

    /**
     * 复制的字节数组给定的OutputStream的内容。
     * Copy the contents of the given byte array to the given OutputStream.
     * Closes the stream when done.
     * @param in the byte array to copy from
     * @param out the OutputStream to copy to
     * @throws IOException in case of I/O errors
     */
    public static void copy(byte[] in, OutputStream out) throws IOException {
        Assert.notNull(in, "No input byte array specified");
        Assert.notNull(out, "No OutputStream specified");
        try {
            out.write(in);
        }
        finally {
            try {
                out.close();
            }
            catch (IOException ex) {
            }
        }
    }

    /**
     * 复制到一个新的给定的InputStream字节数组的内容
     * Copy the contents of the given InputStream into a new byte array.
     * Closes the stream when done.
     * @param in the stream to copy from
     * @return the new byte array that has been copied to
     * @throws IOException in case of I/O errors
     */
    public static byte[] copyToByteArray(InputStream in) throws IOException {
        ByteArrayOutputStream out = new ByteArrayOutputStream(BUFFER_SIZE);
        copy(in, out);
        return out.toByteArray();
    }


    //---------------------------------------------------------------------
    // Copy methods for java.io.Reader / java.io.Writer
    //---------------------------------------------------------------------

    /**
     * 从reader对象拷贝到writer
     * Copy the contents of the given Reader to the given Writer.
     * Closes both when done.
     * @param in the Reader to copy from
     * @param out the Writer to copy to
     * @return the number of characters copied
     * @throws IOException in case of I/O errors
     */
    public static int copy(Reader in, Writer out) throws IOException {
        Assert.notNull(in, "No Reader specified");
        Assert.notNull(out, "No Writer specified");
        try {
            int byteCount = 0;
            char[] buffer = new char[BUFFER_SIZE];
            int bytesRead = -1;
            while ((bytesRead = in.read(buffer)) != -1) {
                out.write(buffer, 0, bytesRead);
                byteCount += bytesRead;
            }
            out.flush();
            return byteCount;
        }
        finally {
            try {
                in.close();
            }
            catch (IOException ex) {
            }
            try {
                out.close();
            }
            catch (IOException ex) {
            }
        }
    }

    /**
     * 将给定字符串的内容复制到给定的输出写入器。
     * Copy the contents of the given String to the given output Writer.
     * Closes the writer when done.
     * @param in the String to copy from
     * @param out the Writer to copy to
     * @throws IOException in case of I/O errors
     */
    public static void copy(String in, Writer out) throws IOException {
        Assert.notNull(in, "No input String specified");
        Assert.notNull(out, "No Writer specified");
        try {
            out.write(in);
        }
        finally {
            try {
                out.close();
            }
            catch (IOException ex) {
            }
        }
    }

    /**
     * 将给定reader的内容复制到字符串中。
     * Copy the contents of the given Reader into a String.
     * Closes the reader when done.
     * @param in the reader to copy from
     * @return the String that has been copied to
     * @throws IOException in case of I/O errors
     */
    public static String copyToString(Reader in) throws IOException {
        StringWriter out = new StringWriter();
        copy(in, out);
        return out.toString();
    }

}
```



