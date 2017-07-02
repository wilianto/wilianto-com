---
layout: post
title: "Read and Write HDFS Using Java"
date: 2016-04-14 16:52:00 +0700
meta_keywords: "read hdfs, write hdfs, hadoop hdfs"
meta_description: "Step by step create read and write to HDFS using Java"
comments: true
---

I use Hadoop for my final project at University. One of the requirement is that write and read to HDFS using Java. In my case, I have to write twitter streaming data to HDFS file.

This is the sample code which I use for read, write, copy from local and delete the file on HDFS using Java. You have to add hadoop client, hadoop core and hadoop hdfs library to your classpath.

{% highlight java %}
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.FSDataInputStream;
import org.apache.hadoop.fs.FSDataOutputStream;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;

public class HDFSOperation {
  protected String hadoopPath;
  protected Configuration conf;
  protected FileSystem fs;
  
  public HDFSOperation(String hadoopPath){
    this.hadoopPath = hadoopPath;
    this.init();
  }
  
  protected void init(){
    conf = new Configuration();
    conf.addResource(new Path(hadoopPath + "/core-site.xml"));
    conf.addResource(new Path(hadoopPath + "/hdfs-site.xml"));
    conf.set("fs.hdfs.impl", org.apache.hadoop.hdfs.DistributedFileSystem.class.getName());
    conf.set("fs.file.impl", org.apache.hadoop.fs.LocalFileSystem.class.getName());
    try {
      fs = FileSystem.get(conf);
    } catch (IOException ex) {
      System.out.println(ex.getMessage());
    }
  }
  
  public void write(String hdfsPath, String content) {
    Path path = new Path(hdfsPath);
    try{
      //delete file if exist
      if(fs.exists(path)){
        fs.delete(path, true);
      }
      
      //create file and write content to file
      FSDataOutputStream fos = fs.create(path);
      fos.writeUTF(content);
      fos.close();
    }catch(IOException ex){
      System.out.println(ex.getMessage());
    }
  }
  
  public String read(String hdfsPath) {
    String content = null;
    Path path = new Path(hdfsPath);
    try{
      //create file and write content to file
      FSDataInputStream fis = fs.open(path);
      BufferedReader reader = new BufferedReader(new InputStreamReader(fis.getWrappedStream()));
      String s;
      while((s = reader.readLine()) != null){
        content += s;
      }
      reader.close();
      fis.close();
    }catch(IOException ex){
      System.out.println(ex.getMessage());
    }
    return content;
  }
  
  public void copyFromLocal(String localPath, String hdfsPath){
    Path srcPath = new Path(localPath);
    Path destPath = new Path(hdfsPath);
    
    try{
      //check is path exist
      if(fs.exists(destPath)){
        System.out.println("No such destination " + destPath);
        return;
      }
      
      fs.copyFromLocalFile(srcPath, destPath);
    }catch(IOException ex){
      System.out.println(ex.getMessage());
    }
  }
  
  public void delete(String hdfsPath) {
    Path path = new Path(hdfsPath);
    try{
      fs.delete(path, true);
    }catch(IOException ex){
      System.out.println(ex.getMessage());
    }
  }
}
{% endhighlight %}

And then make a main class to test it.

{% highlight java %}
public class Main {
  public static void main(String[] args) {
    String hadoopPath = "your-hadoop-path";
    
    HDFSOperation hDFSOperation = new HDFSOperation(hadoopPath);
    hDFSOperation.write("testing.txt", "hello world!");
    System.out.println(hDFSOperation.read("testing-lagi.txt"));
    hDFSOperation.copyFromLocal("/Users/wilianto/data", "data");
    hDFSOperation.delete("testing.txt");
  }
}
{% endhighlight %}

After that open http://localhost:50070/explorer.html to browse your filesystem.

<a href="/assets/images/posts/browsing-hdfs.png"><img src="/assets/images/posts/browsing-hdfs.png" width="100%"></a>

NOTE: Make sure you set `fs.hdfs.impl and fs.file.impl` on your configuration. If you don’t set it, you will get an error `Exception in thread "main" java.io.IOException: No FileSystem for scheme: hdfs` or `Exception in thread "main" java.io.IOException: No FileSystem for scheme: file`.

```
conf.set("fs.hdfs.impl", org.apache.hadoop.hdfs.DistributedFileSystem.class.getName());
conf.set("fs.file.impl", org.apache.hadoop.fs.LocalFileSystem.class.getName());
```

