# Blogger Data Model
```
erDiagram

    USER {
        String id
        String url
        String displayName
        String about
        String imageUrl
        Locale locale
        Datetime created
    }

    BLOG {
        String id
        String url
        String name
        String description
        Int totalPosts
        Int totalPages
        Locale locale
        Datetime published
        Datetime updated   
    }

    PAGE {
        String id
        String url
        String title
        String content
        Datetime published
        Datetime updated
    }

    POST {
        String id
        String url
        String title
        String content
        String customMetaData
        Location location
        Int totalReplies
        Array labels
        Array imagesURLs 
        Datetime published
        Datetime updated     
    }

    COMMENT {
        String id
        String inReplyTo  
        String content   
        Datetime published
        Datetime updated 
    }

    LOCALE {
        String language
        String country
        String variant
    }

    LOCATION {
        String name
        Double latitude
        Double longitude
    }


    BLOG ||--o{ PAGE : ""
    BLOG ||--o{ POST : ""
    
    POST ||--o{ COMMENT : replies
    
    USER ||--o{ BLOG : owns
    USER ||--o{ COMMENT : authors
    USER ||--o{ POST : authors
    USER ||--o{ PAGE : authors


```

[![](https://mermaid.ink/img/eyJjb2RlIjoiZXJEaWFncmFtXG5cbiAgICBVU0VSIHtcbiAgICAgICAgU3RyaW5nIGlkXG4gICAgICAgIFN0cmluZyB1cmxcbiAgICAgICAgU3RyaW5nIGRpc3BsYXlOYW1lXG4gICAgICAgIFN0cmluZyBhYm91dFxuICAgICAgICBTdHJpbmcgaW1hZ2VVcmxcbiAgICAgICAgTG9jYWxlIGxvY2FsZVxuICAgICAgICBEYXRldGltZSBjcmVhdGVkXG4gICAgfVxuXG4gICAgQkxPRyB7XG4gICAgICAgIFN0cmluZyBpZFxuICAgICAgICBTdHJpbmcgdXJsXG4gICAgICAgIFN0cmluZyBuYW1lXG4gICAgICAgIFN0cmluZyBkZXNjcmlwdGlvblxuICAgICAgICBJbnQgdG90YWxQb3N0c1xuICAgICAgICBJbnQgdG90YWxQYWdlc1xuICAgICAgICBMb2NhbGUgbG9jYWxlXG4gICAgICAgIERhdGV0aW1lIHB1Ymxpc2hlZFxuICAgICAgICBEYXRldGltZSB1cGRhdGVkICAgXG4gICAgfVxuXG4gICAgUEFHRSB7XG4gICAgICAgIFN0cmluZyBpZFxuICAgICAgICBTdHJpbmcgdXJsXG4gICAgICAgIFN0cmluZyB0aXRsZVxuICAgICAgICBTdHJpbmcgY29udGVudFxuICAgICAgICBEYXRldGltZSBwdWJsaXNoZWRcbiAgICAgICAgRGF0ZXRpbWUgdXBkYXRlZFxuICAgIH1cblxuICAgIFBPU1Qge1xuICAgICAgICBTdHJpbmcgaWRcbiAgICAgICAgU3RyaW5nIHVybFxuICAgICAgICBTdHJpbmcgdGl0bGVcbiAgICAgICAgU3RyaW5nIGNvbnRlbnRcbiAgICAgICAgU3RyaW5nIGN1c3RvbU1ldGFEYXRhXG4gICAgICAgIExvY2F0aW9uIGxvY2F0aW9uXG4gICAgICAgIEludCB0b3RhbFJlcGxpZXNcbiAgICAgICAgQXJyYXkgbGFiZWxzXG4gICAgICAgIEFycmF5IGltYWdlc1VSTHMgXG4gICAgICAgIERhdGV0aW1lIHB1Ymxpc2hlZFxuICAgICAgICBEYXRldGltZSB1cGRhdGVkICAgICBcbiAgICB9XG5cbiAgICBDT01NRU5UIHtcbiAgICAgICAgU3RyaW5nIGlkXG4gICAgICAgIFN0cmluZyBpblJlcGx5VG8gIFxuICAgICAgICBTdHJpbmcgY29udGVudCAgIFxuICAgICAgICBEYXRldGltZSBwdWJsaXNoZWRcbiAgICAgICAgRGF0ZXRpbWUgdXBkYXRlZCBcbiAgICB9XG5cbiAgICBMT0NBTEUge1xuICAgICAgICBTdHJpbmcgbGFuZ3VhZ2VcbiAgICAgICAgU3RyaW5nIGNvdW50cnlcbiAgICAgICAgU3RyaW5nIHZhcmlhbnRcbiAgICB9XG5cbiAgICBMT0NBVElPTiB7XG4gICAgICAgIFN0cmluZyBuYW1lXG4gICAgICAgIERvdWJsZSBsYXRpdHVkZVxuICAgICAgICBEb3VibGUgbG9uZ2l0dWRlXG4gICAgfVxuXG5cbiAgICBCTE9HIHx8LS1veyBQQUdFIDogXCJcIlxuICAgIEJMT0cgfHwtLW97IFBPU1QgOiBcIlwiXG4gICAgXG4gICAgUE9TVCB8fC0tb3sgQ09NTUVOVCA6IHJlcGxpZXNcbiAgICBcbiAgICBVU0VSIHx8LS1veyBCTE9HIDogb3duc1xuICAgIFVTRVIgfHwtLW97IENPTU1FTlQgOiBhdXRob3JzXG4gICAgVVNFUiB8fC0tb3sgUE9TVCA6IGF1dGhvcnNcbiAgICBVU0VSIHx8LS1veyBQQUdFIDogYXV0aG9yc1xuXG4iLCJtZXJtYWlkIjp7fSwidXBkYXRlRWRpdG9yIjpmYWxzZX0)](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoiZXJEaWFncmFtXG5cbiAgICBVU0VSIHtcbiAgICAgICAgU3RyaW5nIGlkXG4gICAgICAgIFN0cmluZyB1cmxcbiAgICAgICAgU3RyaW5nIGRpc3BsYXlOYW1lXG4gICAgICAgIFN0cmluZyBhYm91dFxuICAgICAgICBTdHJpbmcgaW1hZ2VVcmxcbiAgICAgICAgTG9jYWxlIGxvY2FsZVxuICAgICAgICBEYXRldGltZSBjcmVhdGVkXG4gICAgfVxuXG4gICAgQkxPRyB7XG4gICAgICAgIFN0cmluZyBpZFxuICAgICAgICBTdHJpbmcgdXJsXG4gICAgICAgIFN0cmluZyBuYW1lXG4gICAgICAgIFN0cmluZyBkZXNjcmlwdGlvblxuICAgICAgICBJbnQgdG90YWxQb3N0c1xuICAgICAgICBJbnQgdG90YWxQYWdlc1xuICAgICAgICBMb2NhbGUgbG9jYWxlXG4gICAgICAgIERhdGV0aW1lIHB1Ymxpc2hlZFxuICAgICAgICBEYXRldGltZSB1cGRhdGVkICAgXG4gICAgfVxuXG4gICAgUEFHRSB7XG4gICAgICAgIFN0cmluZyBpZFxuICAgICAgICBTdHJpbmcgdXJsXG4gICAgICAgIFN0cmluZyB0aXRsZVxuICAgICAgICBTdHJpbmcgY29udGVudFxuICAgICAgICBEYXRldGltZSBwdWJsaXNoZWRcbiAgICAgICAgRGF0ZXRpbWUgdXBkYXRlZFxuICAgIH1cblxuICAgIFBPU1Qge1xuICAgICAgICBTdHJpbmcgaWRcbiAgICAgICAgU3RyaW5nIHVybFxuICAgICAgICBTdHJpbmcgdGl0bGVcbiAgICAgICAgU3RyaW5nIGNvbnRlbnRcbiAgICAgICAgU3RyaW5nIGN1c3RvbU1ldGFEYXRhXG4gICAgICAgIExvY2F0aW9uIGxvY2F0aW9uXG4gICAgICAgIEludCB0b3RhbFJlcGxpZXNcbiAgICAgICAgQXJyYXkgbGFiZWxzXG4gICAgICAgIEFycmF5IGltYWdlc1VSTHMgXG4gICAgICAgIERhdGV0aW1lIHB1Ymxpc2hlZFxuICAgICAgICBEYXRldGltZSB1cGRhdGVkICAgICBcbiAgICB9XG5cbiAgICBDT01NRU5UIHtcbiAgICAgICAgU3RyaW5nIGlkXG4gICAgICAgIFN0cmluZyBpblJlcGx5VG8gIFxuICAgICAgICBTdHJpbmcgY29udGVudCAgIFxuICAgICAgICBEYXRldGltZSBwdWJsaXNoZWRcbiAgICAgICAgRGF0ZXRpbWUgdXBkYXRlZCBcbiAgICB9XG5cbiAgICBMT0NBTEUge1xuICAgICAgICBTdHJpbmcgbGFuZ3VhZ2VcbiAgICAgICAgU3RyaW5nIGNvdW50cnlcbiAgICAgICAgU3RyaW5nIHZhcmlhbnRcbiAgICB9XG5cbiAgICBMT0NBVElPTiB7XG4gICAgICAgIFN0cmluZyBuYW1lXG4gICAgICAgIERvdWJsZSBsYXRpdHVkZVxuICAgICAgICBEb3VibGUgbG9uZ2l0dWRlXG4gICAgfVxuXG5cbiAgICBCTE9HIHx8LS1veyBQQUdFIDogXCJcIlxuICAgIEJMT0cgfHwtLW97IFBPU1QgOiBcIlwiXG4gICAgXG4gICAgUE9TVCB8fC0tb3sgQ09NTUVOVCA6IHJlcGxpZXNcbiAgICBcbiAgICBVU0VSIHx8LS1veyBCTE9HIDogb3duc1xuICAgIFVTRVIgfHwtLW97IENPTU1FTlQgOiBhdXRob3JzXG4gICAgVVNFUiB8fC0tb3sgUE9TVCA6IGF1dGhvcnNcbiAgICBVU0VSIHx8LS1veyBQQUdFIDogYXV0aG9yc1xuXG4iLCJtZXJtYWlkIjp7fSwidXBkYXRlRWRpdG9yIjpmYWxzZX0)
