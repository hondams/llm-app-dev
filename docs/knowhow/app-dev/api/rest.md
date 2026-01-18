# REST

## REST Architecture

- https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.2.0.md
- https://www.ibm.com/think/topics/rest-apis
- https://restfulapi.net/rest-architectural-constraints/
- https://en.wikipedia.org/wiki/REST
- https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design
- https://www.baeldung.com/cs/rest-architecture


## URI設計

- [GoogleのWebAPI設計とWebAPI設計のベストプラクティスを比較してみる](https://qiita.com/howdy39/items/3b2b14ce73ec44c54f7b)
- [翻訳: WebAPI 設計のベストプラクティス](https://qiita.com/mserizawa/items/b833e407d89abd21ee72)
- https://docs.cloud.google.com/apis/design?hl=ja
- https://serverless.co.jp/blog/6zp8gbrwmsu/


## Open API

- https://openapi-generator.tech/docs/configuration/
  - Hmoe
- https://github.com/OpenAPITools/openapi-generator/tree/master/modules/openapi-generator-maven-plugin
  - openapi-generator-maven-plugin
- ソース
  - https://github.com/OpenAPITools/openapi-generator/blob/master/modules/openapi-generator/src/main/java/org/openapitools/codegen/languages/AbstractJavaCodegen.java
  - https://github.com/OpenAPITools/openapi-generator/blob/master/modules/openapi-generator/src/main/java/org/openapitools/codegen/languages/SpringCodegen.java
- springdoc-openapi-starter-webmvc-ui
  - openapi-*.yaml を src/main/resources/static に置くと自動で /openapi-*.yaml で配信されます。
  - http://localhost:8080/swagger-ui/index.html
