discovery
=========

A hyper-schema written in YAML.

```yml
# .discovery-v2.1.0.yml

service:
  id: com.example.hoge_api
  url: http://example.com/api/v2

endpoint:
  ListEntry:
    path: GET /entries
    params:
      page?: Integer
    response: [Entry]
  PostEntry:
  	path: POST /entries
  	body: Entry
  GetEntry:
  	path: GET /entries/{id}
  	response: Entry
  PatchEntry:
  	path: PATCH /entries/{id}
  	body: Entry

entity:
  Entry:
  	id: Key
  	title: String
  	body: String

error:
  kind: String
  message: String
  url: String
```

Code Generation
---

### Objective-C Client

```objc
@interface HogeAPI : DCVAPIBase

+ (instancetype)APIWithConfiguration:(HogeAPIConfiguration *)configuration;

/**
 *  List entries.
 *
 *  @param success Called on success.
 *  @param failure Called on failure.
 *
 *  @return API call object
 */
- (DCVAPICall *)listEntry:(void(^)(NSArray *entries))success failure:(void(^)(NSError *error))failure;

/**
 *  List entries.
 *
 *  @param page Page
 *  @param success Called on success.
 *  @param failure Called on failure.
 *
 *  @return API call object
 */
- (DCVAPICall *)listEntryWithPage:(NSInteger)page success:(void(^)(NSArray *entries))success failure:(void(^)(NSError *error))failure;

/**
 *  Post an entry.
 *
 *  @param entry An Entry
 *  @param success Called on success.
 *  @param failure Called on failure.
 *
 *  @return API call object
 */
- (DCVAPICall *)postEntry:(HogeAPIEntry *)entry success:(void(^)())success failure:(void(^)(NSError *error))failure

/**
 *  Get an entry.
 *
 *  @param identifier /entries/{id}
 *  @param success Called on success.
 *  @param failure Called on failure.
 *
 *  @return API call object
 */
- (DCVAPICall *)getEntry:(NSString *)identifier success:(void(^)(HogeAPIEntry *)entry)success failure:(void(^)(NSError *error))failure;

/**
 *  Patch an entry.
 *
 *  @param identifier /entries/{id}
 *  @param entry An entry
 *  @param success Called on success.
 *  @param failure Called on failure.
 *
 *  @return API call object
 */
- (DCVAPICall *)patchEntry:(NSString *)identifier entry:(HogeAPIEntry *)entry success:(void(^)())success failure:(void(^)(NSError *error))failure;

@end
```

```objc
@interface HogeAPIEntry : DCVObject <NSCopying, NSCoding>

@property (nonatomic, copy) NSString *identifier;
@property (nonatomic, copy) NSString *title;
@property (nonatomic, copy) NSString *body;

@end
```

- Documentation with appledoc
- Backboned by AFNetworking & Mantle
- `NSError` binding from a response

### Documentation Generation

Generate a documentation backed by Swagger.

### Mock Server Generation

Generate a mock server.

`dcv s -p 3000`

### Stub Generation

Generate a stub code backed by `OHHTTPStubs` for iOS.

```objc
[HogeAPIStubs stubAll];

[HogeAPIStubs clearAllStubs];
```

Considered Features
---

- Support for custom serializer such as msgpack, protobuf
- Generate & modify a schema file with command line
- Client-side validation
- Java bean generation
- From / to JSON-Schema translation
