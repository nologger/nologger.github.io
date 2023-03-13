---
title: file upload
category: spring
layout: post
---

> spring file upload

***

1. 초기설정
2. file upload form
3. file process

***

# 1. 초기설정

## 1.1 의존성 주입
`build.gradle`
```gradle
...

dependencies {
    implementation 'commons-fileupload:commons-fileupload:1.5'
    implementation 'commons-io:commons-io:2.11.0'
}

...
```

## 1.2 MultipartResolver 설정
`servlet-context.xml`
```xml
    <!-- ... -->

    <beans:bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <beans:property name="maxUploadSize" value="100000"/>
        <beans:property name="defaultEncoding" value="utf-8"/>
        <beans:property name="uploadTempDir" ref="uploadDirResource"/>
    </beans:bean>

    <beans:bean id="uploadDirResource" class="org.springframework.core.io.FileSystemResource">
        <beans:constructor-arg value="c:/upload/">
    </beans:bean>

    <!-- ... -->
```

# 2. file upload form
```html
<form method="POST" enctype="multipart/form-data">
    <input type="file" name="fileUploadTest ">
</form>
```

`FileController.java`
```java
@Controller
public class FileController {
    class Sample {
        private MultipartFile file;

        public MultipartFile getFile() {
            return this.file;
        }

        public void setFile(MultipartFile multipartFile) {
            this.file = multipartFile;
        }
    }

    @PostMapping("/sample1")
    public String sampleForm1(
        @RequestParam("sampleFile") MultipartFile file
    ) {
        String filename = file.getOriginalFileName();
        File f = new File("/temp/upload/" + filename);

        try {
            file.transferTo(f);
        } catch (IOException e) {
            e.printStackTrace();
        }

        return "sample";
    }

    @PostMapping("/sample2")
    public String sampleForm2(
        MultipartHttpServletRequest request
    ) {
        MultipartFile file = requeste.getFile("sampleFile");
        String filename = file.getOriginalFileName();
        File f = new File("/temp/upload/" + filename);

        try {
            file.transferTo(f);
        } catch (IOException e) {
            e.printStackTrace();
        }
        return "sample";
    }

    @PostMapping("/sample3")
    public String sampleForm2(
        @ModelAttribute("sample") Sample sample
    ) {
        MultipartFile file = sample.getFile();
        String filename = file.getOriginalFileName();
        File f = new File("/temp/upload/" + filename);

        try {
            file.transferTo(f);
        } catch (IOException e) {
            e.printStackTrace();
        }
        return "sample";
    }
}
```