## ��java��ô������һ���ļ�������ļ�д�ı�����

### �ʣ���java����򵥵Ĵ����ļ�д�ļ��ķ�����ʲô

### ��Ѵ�:
����һ���ı��ļ���ע�⣺������ļ����ڣ���Ḳ�Ǹ��ļ���
````java
PrintWriter writer = new PrintWriter("the-file-name.txt", "UTF-8");
writer.println("The first line");
writer.println("The second line");
writer.close();
````
����һ��λ�������ļ���ͬ���Ḳ�����ļ���
````java
byte data[] = ...
FileOutputStream out = new FileOutputStream("the-file-name");
out.write(data);
out.close();
````

Java 7+ �û�������[`File`](http://docs.oracle.com/javase/7/docs/api/index.html?java/nio/file/Files.html)����д�ļ�
����һ���ı��ļ�
````java
List<String> lines = Arrays.asList("The first line", "The second line");
Path file = Paths.get("the-file-name.txt");
Files.write(file, lines, Charset.forName("UTF-8"));
//Files.write(file, lines, Charset.forName("UTF-8"), StandardOpenOption.APPEND);
````
����һ���������ļ�
````java
byte data[] = ...
Path file = Paths.get("the-file-name");
Files.write(file, data);
//Files.write(file, data, StandardOpenOption.APPEND);
````

### �����Ĵ𰸣�1��:
��Java 7+��
````java
try (Writer writer = new BufferedWriter(new OutputStreamWriter(
              new FileOutputStream("filename.txt"), "utf-8"))) {
   writer.write("something");
}
````
����һЩʵ�õķ������£�
* [`FileUtils.writeStringtoFile(..)`](https://commons.apache.org/proper/commons-io/apidocs/org/apache/commons/io/FileUtils.html#writeStringToFile%28java.io.File,%20java.lang.String,%20java.nio.charset.Charset%29) ������ commons-io ��
* [`Files.write(..)`](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/io/Files.html#write%28java.lang.CharSequence,%20java.io.File,%20java.nio.charset.Charset%29) ������ guava
Note also that you can use a FileWriter, but it uses the default encoding, 
which is often a bad idea - it's best to specify the encoding explicitly.
��Ҫע�����ʹ�� `FileWriter`��������ʹ�õ���Ĭ�ϱ��룬�ⲻ�Ǻܺõķ������������ȷָ������


������������prior-to-java-7��ԭʼ����
````java
Writer writer = null;

try {
    writer = new BufferedWriter(new OutputStreamWriter(
          new FileOutputStream("filename.txt"), "utf-8"));
    writer.write("Something");
} catch (IOException ex) {
  // report
} finally {
   try {writer.close();} catch (Exception ex) {/*ignore*/}
}
````
���Կ�[`Reading, Writing, and Creating Files`](http://docs.oracle.com/javase/tutorial/essential/io/file.html)(����NIO2)

### �����𰸣�2����
````java
public class Program {
    public static void main(String[] args) {
        String text = "Hello world";
        BufferedWriter output = null;
        try {
            File file = new File("example.txt");
            output = new BufferedWriter(new FileWriter(file));
            output.write(text);
        } catch ( IOException e ) {
            e.printStackTrace();
        } finally {
            if ( output != null ) output.close();
        }
    }
}
````

### �����𰸣�3����
����Ѿ�����Ҫд���ļ��е����ݣ�[`java.nio.file.Files`](https://docs.oracle.com/javase/7/docs/api/java/nio/file/Files.html) ��Ϊ Java 7 ���Ӳ��ֵ�native I/O���ṩ�˼򵥸�Ч�ķ�����ʵ�����Ŀ��

�����ϴ����ļ���д�ļ�ֻ��Ҫһ�У�������ֻ��һ���������ã�
��������Ӵ�������д��6����ͬ���ļ���չʾ����ôʹ�õ�

````java
Charset utf8 = StandardCharsets.UTF_8;
List<String> lines = Arrays.asList("1st line", "2nd line");
byte[] data = {1, 2, 3, 4, 5};

try {
    Files.write(Paths.get("file1.bin"), data);
    Files.write(Paths.get("file2.bin"), data,
            StandardOpenOption.CREATE, StandardOpenOption.APPEND);
    Files.write(Paths.get("file3.txt"), "content".getBytes());
    Files.write(Paths.get("file4.txt"), "content".getBytes(utf8));
    Files.write(Paths.get("file5.txt"), lines, utf8);
    Files.write(Paths.get("file6.txt"), lines, utf8,
            StandardOpenOption.CREATE, StandardOpenOption.APPEND);
} catch (IOException e) {
    e.printStackTrace();
}
````

### �����𰸣�4����
������һ��С������������д�ļ����ð汾�Ĵ���Ƚϳ������ǿ����������
````java
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.Writer;

public class writer {
    public void writing() {
        try {
            //Whatever the file path is.
            File statText = new File("E:/Java/Reference/bin/images/statsTest.txt");
            FileOutputStream is = new FileOutputStream(statText);
            OutputStreamWriter osw = new OutputStreamWriter(is);    
            Writer w = new BufferedWriter(osw);
            w.write("POTATO!!!");
            w.close();
        } catch (IOException e) {
            System.err.println("Problem writing to the file statsTest.txt");
        }
    }

    public static void main(String[]args) {
        writer write = new writer();
        write.writing();
    }
}
````




stackoverflow���ӣ�
http://stackoverflow.com/questions/2885173/how-to-create-a-file-and-write-to-a-file-in-java
