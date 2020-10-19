---
layout: post
title:  "[Vue.js] 자바스크립트 힙메모리 부족 ineffective mark-compacts near heap limit allocation failed - javascript heap out of memory vue"
categories: front
comments: true



---



vue를 빌드하는데 갑자기 

```javascript
[10764:00000000000E1740]    55769 ms: Mark-sweep 1367.5 (1420.9) -> 1363.0 (1416.4) MB, 630.8 / 0.0 ms  (average mu = 0.110, current mu = 0.023) allocation failure GC in old space requested
[10764:00000000000E1740]    56390 ms: Mark-sweep 1363.0 (1416.4) -> 1363.0 (1416.4) MB, 621.4 / 0.0 ms  (average mu = 0.058, current mu = 0.000) allocation failure GC in old space requested


<--- JS stacktrace --->

==== JS stack trace =========================================

    0: ExitFrame [pc: 000002279FB5C5C1]
Security context: 0x00d940d9e6e9 <JSObject>
    1: /* anonymous */ [000003F3C143A579] [C:\DEV\vue-floor\node_modules\webpack-sources\node_modules\source-map\lib\source-node.js:~174] [pc=00000227A05799A9](this=0x00936c3bcc09 <SourceNode map = 000003D553803C21>,chunk=0x023a7576abc1 <SourceNode map = 000003D553803C21>)
    2: arguments adaptor frame: 3->1
    3: SourceNode_add [000001DC621690C9] [C:\DE...

FATAL ERROR: Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory
 1: 000000013F4EDD8A v8::internal::GCIdleTimeHandler::GCIdleTimeHandler+4506
 2: 000000013F4C8886 node::MakeCallback+4534
 3: 000000013F4C9200 node_module_register+2032
 4: 000000013F7E30DE v8::internal::FatalProcessOutOfMemory+846
 5: 000000013F7E300F v8::internal::FatalProcessOutOfMemory+639
 6: 000000013F9C9804 v8::internal::Heap::MaxHeapGrowingFactor+9620
 7: 000000013F9C07E6 v8::internal::ScavengeJob::operator=+24550
 8: 000000013F9BEE3C v8::internal::ScavengeJob::operator=+17980
 9: 000000013F9BE305 v8::internal::ScavengeJob::operator=+15109
10: 000000013F9C7C64 v8::internal::Heap::MaxHeapGrowingFactor+2548
11: 000000013FAF1CD8 v8::internal::Factory::AllocateRawArray+56
12: 000000013FAF2652 v8::internal::Factory::NewFixedArrayWithFiller+66
13: 000000013FB1C896 v8::internal::Factory::NewCallHandlerInfo+113510
14: 000000013FE6EC70 v8::internal::compiler::JSCallReducer::simplified+29264
15: 000002279FB5C5C1
npm ERR! code ELIFECYCLE
npm ERR! errno 134
npm ERR! vue-project@0.1.0 build: `vue-cli-service build`
npm ERR! Exit status 134
npm ERR!
npm ERR! Failed at the vue-bidpro-floor@0.1.0 build script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\sora\AppData\Roaming\npm-cache\_logs\2020-08-14T03_29_36_985Z-debug.log
```



이건 또 뭐야.... 힙메모리가 부족하단다

예전에는 자바에서 이런문제가 있어서 메모리 늘려줬는데, js도 있구낭..

`vue-cli`에서 [issue](https://github.com/vuejs/vue-cli/issues/1453)에 등록되어있던데 이건 vue-cli문제가 아니고 nodejs문제라고 한다.



여튼

```javascript
"scripts": {
    "serve": "vue-cli-service serve",
    "build": "node --max_old_space_size=4096 node_modules/@vue/cli-service/bin/vue-cli-service.js build --open",
    "lint": "vue-cli-service lint"
  },
```

이렇게 `node --max_old_space_size=4096 node_modules/@vue/cli-service/bin/vue-cli-service.js build --open` 로 바꿔주니 빌드성공!

