date: "2016-07-01"
categories: 
  - spring
title: Spring源码分析之Resource
tags : 
 - spring
---


基于Spring1.0


```java

public interface InputStreamSource {

    InputStream getInputStream() throws IOException;

}

public interface Resource extends InputStreamSource {
    boolean exists();
    boolean isOpen();
    URL getURL() throws IOException;
    File getFile() throws IOException;
    String getDescription();
}


public abstract class AbstractResource implements Resource {

    protected static final String URL_PROTOCOL_FILE = "file";

    @Override
    public boolean exists() {
        try {
            return getFile().exists();
        }
        catch (IOException ex) {
            try {
                InputStream is = getInputStream();
                is.close();
                return true;
            }
            catch (IOException ex2) {
                return false;
            }
        }
    }
    
    @Override
    public boolean isOpen() {
        return false;
    }
    /**
    * 如果实现类重写该方法,则使用子类,如果没有重写,则表示子类不支持该方法,抛出异常
    */
    @Override
    public URL getURL() throws IOException {
        throw new FileNotFoundException(getDescription() + " cannot be resolved to URL");
    }

    /**
    * 如果实现类重写该方法,则使用子类,如果没有重写,则表示子类不支持该方法,抛出异常
    */
    @Override
    public File getFile() throws IOException {
        throw new FileNotFoundException(getDescription() + " cannot be resolved to absolute file path");
    }

    
    @Override
    public String toString() {
        return getDescription();
    }

    
    @Override
    public boolean equals(Object obj) {
        return (obj instanceof Resource && ((Resource) obj).getDescription().equals(getDescription()));
    }

    
    @Override
    public int hashCode() {
        return getDescription().hashCode();
    }

}



public class ClassPathResource extends AbstractResource {

    private final String path;

    private Class clazz;

   
    public ClassPathResource(String path) {
        // /开头表示绝对路径
        if (path.startsWith("/")) {
            path = path.substring(1);
        }
        this.path = path;
    }

    
    public ClassPathResource(String path, Class clazz) {
        this.path = path;
        this.clazz = clazz;
    }

    @Override
    public InputStream getInputStream() throws IOException {
        InputStream is = null;
        if (this.clazz != null) {
            is = this.clazz.getResourceAsStream(this.path);
        } else {
            ClassLoader ccl = Thread.currentThread().getContextClassLoader();
            is = ccl.getResourceAsStream(this.path);
        }
        if (is == null) {
            throw new FileNotFoundException("Could not open " + getDescription());
        }
        return is;
    }

    @Override
    public URL getURL() throws IOException {
        URL url = null;
        if (this.clazz != null) {
            url = this.clazz.getResource(this.path);
        } else {
            ClassLoader ccl = Thread.currentThread().getContextClassLoader();
            url = ccl.getResource(this.path);
        }
        if (url == null) {
            throw new FileNotFoundException(getDescription() + " cannot be resolved to URL " +
                    "because it does not exist");
        }
        return url;
    }

    @Override
    public File getFile() throws IOException {
        URL url = getURL();
        if (!URL_PROTOCOL_FILE.equals(url.getProtocol())) {
            throw new FileNotFoundException(getDescription() + " cannot be resolved to absolute file path " +
                    "because it does not reside in the file system: URL=[" + url + "]");
        }
        return new File(URLDecoder.decode(url.getFile()));
    }

    @Override
    public String getDescription() {
        return "class path resource [" + this.path + "]";
    }

}



```

AbstractResource主要实现了两个方法，exists()和 isOpen()，   exists方法先通过文件判断资源是否存在，然后根据InputStream进行判断。isOpen()返回false


getURL() ,getFile() 在这里不是抽象方法的原因是因为： 如果实现类重写该方法,则使用子类,如果没有重写,则表示子类不支持该方法,直接抛出异常，避免在实现类中，命名和该方法没关联，还需要抛出异常

比如ClassPathResource重写， InputStreamReSource


