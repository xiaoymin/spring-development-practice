`StreamUtils`是一些流处理操作,主要公开如下方法：

* `public static byte[] copyToByteArray(InputStream in)`:复制到一个新的给定的InputStream字节数组的内容

* `public static String copyToString(InputStream in,Charset charset)`:复制输入流内容给String字符串

* `public static void copy(byte[] in, OutputStream out)`:复制字节数组给定的OutputStream的内容。
* `public static void copy(String in, Charset charset, OutputStream out)`:复制输入流字节内容给string字符串
* `public static int copy(InputStream in, OutputStream out)`:输入流复制到输出流
* `public static long copyRange(InputStream in, OutputStream out, long start, long end)`:复制输入流区间值到输出流


