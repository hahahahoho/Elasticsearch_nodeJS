# Elasticsearch_nodejs

안녕하세요? 엘라스틱서치 2번째 시간입니다. 이전 시간에서는 엘라스틱서치 개념에 대한 이야기를 했었습니다. 혹시 이전 설명을 못 보신 분은 아래 링크를 클릭하여 보고 오시면 더욱 좋을 것 같습니다.

https://github.com/hahahahoho/ElasticSearch_basic

엘라스틱서치를 사용하는 방법은 여러가지가 있습니다. 저는 그 중에서 nodejs를 통해 엘라스틱서치 클라이언트를 다운받고 사용하는 법에 대해서 설명드리도록 하겠습니다.

메서드들에 설명은 엘라스틱홈페이지 공식문서를 최대한 참고하였으며, 이해를 돕기 위해 제 의견이 들어가 있는 점 미리 양해바랍니다.

위에 js폴더를 들어가시면 총 3개의 파일이 있습니다

1. elasticsearch_init
2. elasticsearch_mapping
3. elasticsearch_serach

순서대로 보시면서 글을 읽으시면 도움이 될 것이라 생각합니다.

먼저, nodejs에서 사용하기 위해 nodejs 프로젝트를 만들고 npm명령어를 통해 elasticsearch 클라이언트 모듈을 설치해줍니다. 명령어는 아래와 같습니다.

` npm install elasticsearch --save`

그리고 elasitcsearch를 실제 프로젝트에서 불러오도록 하겠습니다

```
const elasticsearch = require('elasticsearch');
const el_cilent = new elasticsearch.Client({host : 'host주소:9200', log : 'trace'});
```

이제 준비는 다되었습니다. 쉽지요!?? ㅎㅎ

그러나 시작은 이제부터랍니다..... 이제 준비가 되었으면 위에 말한 3개의 파일을 차례대로 설명하면서 이해를 돕도록 하겠습니다.

#### init파일은 index, document를 생성, 조회, 삭제, 수정 방법에 대해서 다룹니다.

파일 주석으로 최대한 이해하기 쉽게 설명해 놓았으니 한 번 읽어보시면 될 거 같습니다.

#### mapping파일은 index에 대해 초기 setting과 mapping설정 관련 내용들과 옵션을 다루고 있습니다. 

첫째, setting을 통해서 인덱싱 성능을 최적화하거나 한글 형태소분석기인(nori) 에 대해서 정의할 수 있습니다. 기본적으로 엘라스틱서치가 제공해주는 setting환경 또한 잘 되어 있어 크게 설정을 변경할 필요는 없으나 프로젝트 환경에 맞춰 더 빠른 인덱싱을 할 때 옵션들을 공부해서 바꿔주시면 되겠습니다. nori 한글형태소분석기의 경우는 한글을 사용하는 웹사이트에서는 거의 필수적으로 사용할 거 같으니 파일에 나와있는대로 설정해서 사용하시면 됩니다.

#####    인덱싱 성능 최적화에 대해 잘 설명해준 사이트 링크 첨부합니다 

   주소 : https://brunch.co.kr/@alden/37

둘째, mapping은 RDBMS에서 스키마의 개념과 같다고 보시면 됩니다. 필드의 type을 설정하고 실제로 어떤 분석기(저희는 nori분석기를 사용하겠죠???)를 사용해서 데이터를 인덱싱하고 검색할 것 인지 미리 정의해 놓는것을 의미합니다. mapping이 중요한 이유에 대해서 몇가지 말씀드리겠습니다. mapping이 없을 경우, 엘라스틱서치는 데이터를 입력할 때 자체적으로 분석하여 type을 지정하게 됩니다. 예를들면 1234의 경우 우리는 문자열로 넣고 싶었으나 엘라스틱서치는 숫자인 integer타입으로 분석하고 넣게 됩니다. 그리고 후에 우리가 문자열을 넣게되면 당연히 에러가 발생하겠죠?? 또한 나중에 키바나를 통해 데이터를 분석하고 시각적으로 보여주기 위해서도 사용됩니다. geo_point와 같은 타입이나 date타입이나 아니면 다른 타입이든 원하고자 하는 타입으로 지정해놔야 추후 키바나에서 데이터를 뽑아서 보여줄 때 원하는 결과가 나오겠죠??

그리고 mapping시 주의해야 할 점은 한 번 데이터가 mapping에 맞게 생성되면 변경할 수 없습니다.

예를들어 name : hahahahoho 데이터를 넣었는데, name : {type : text}로 지정되어 있다면 이를 나중에 바꾸고 싶어도 바꿀 수 없습니다. 에러가 떠버리니 정하실 떄는 꼭 신중하게 타입을 정하셔야 합니다. 그러나 만약 데이터를 넣기 전 새로운 필드를 추가하는 경우는 가능합니다. age : {type : integer}를 기존 mapping밑에 추가하여 넣게되면 정상적으로 mapping이 수정되는 것을 확인할 수 있을 것 입니다.

최대한 쉽고 짧게 설명드리고 싶었는데.... 글로 쓰려니 정말 어렵네요.. ㅜ 아무튼 mapping파일을 열어보면 기본적으로 사용방법과 그 옆에 주석으로 간단하게 설명을 달아놨으니 분석해서 사용하시면  될 것 같습니다. 도움이 되길 바랄게요 ㅜㅜ...

#### 마지막으로 search파일은 검색시 사용되는 다양한 옵션들에 대해서 최대한 설명을 달아놓은 파일입니다.

엘라스틱서치를 사용하는 가장 중요한 이유가 서치인 만큼 옵션이 정말 다양합니다. 그리고 어떻게 조합하느냐에 따라 무궁무진하게 사용이 가능하니 제가 써놓은 옵션 뿐 아니라 다른 옵션도 찾아보시면서 공부하시길 권장드립니다. 가장 기본적으로 사용하는 옵션들에 대해 간략히 설명을 써놓은 거니까 참고용으로만 사용하시면 될 거 같습니다 ^^. 

옵션을 분석하고 설명을 하기도 했지만.... 저 역시 제대로 된 사용을 못하는 거 같으니....ㅜㅜ 같이 공부합시다~!!... ㅎㅎ



글이 마치 주저리주저리 떠드는 것처럼 맥락이 없는 것 같아 마음이 아프네요... 그렇지만 제가 강사는 아니니까 스스로 위안 삼겠습니다.. ㅎㅎ 어떤분에게는 도움이 될 거라는 긍정적인 마음을 갖고서..!!!!이만 글을 마치도록 하겠습니다.  

아! 그리고 다음에 기회가 된다면 RDBMS와 엘라스틱서치를 연동하여 사용하고 실제 웹에서 검색할 수 있는 기능이 구현되면 구현 예제도 업데이트 할 예정이니 관심있으시면 나중에 한 번 찾아와서 보시면 매우 감사드리겠습니다.

