---
layout: post
title: 만약 헤밍웨이가 자바스크립트로 코딩한다면
---

* [만약 헤밍웨이가 자바스크립트로 코딩한다면](http://book.daum.net/detail/book.do?bookid=KOR9788968484742)
* 한빛미디어 리뷰어에 신청했는데, 당첨이 되고 나서 받은 책이라 읽게 되었다. 자바스크립트는 정말 기초만 알고, 사용하지도 않기 때문에 내가 책을 선택할 수 있었다면 아마 하지 않았을 것이다. 게다가 일종의 편견이긴 하지만, 개발 서적을 개발자가 아니라 번역가가 하는 경우(이 책의 저자는 그래도 좀 나은 게 개발자 영어라는 페이스북 그룹을 운영하는 분으로 나프다에도 출연)는 보통 기술적으로 오류가 있는 경우가 많아 좋아하지 않기도 한다.
* 하지만, 왠지 폼 나보이는 책 제목에, 리뷰를 써야 한다는 의무감이 겹쳐 읽기 시작했는데, 의외로 금방 읽을 수 있었다. 빨리 읽을 수 있었던 이유는 유명한 작가들에 관한 이야기가 재미있기도 하고, 자바스크립트를 모르니, 코드를 읽기는 하지만, 이해가 안 되는 부분을 하나씩 분석해볼 시간이 없어 그냥 읽고 넘겼기 때문이다.
  * 코드에 관심이 있다면 [If Hemingway Wrote JavaScript: Explained.](https://javascriptweblog.wordpress.com/2015/01/05/if-hemingway-wrote-javascript-explained)
* 저자는 문학에 대해 관심이 많고, 아주 잘 아는 거 같다. 코드를 보면 자바스크립트를 모르는 사람이라도, 정말 다양한 스타일로 썼다는 걸 알 수 있다. 하지만 문제는 독자도 잘 알아야 이해할 수 있다는 점이 문제이다. 자바스크립트를 사용하며(이런 개발자는 많겠지), 동시에 문학에 대해 잘 알고(대폭 감소), 작가의 스타일을 이해하며(대폭 감소), 그 미묘한 문학적 차이까지 이해하려면 과연 얼마나 남을까? 게다가 이건 번역서라 원래는 해당 작가의 언어로 책을 읽어야 한다는 걸 생각하면...
  * 이건 일단 알고 모르고를 떠나 현업에서 정말 보기 싫은 코드 스타일... [https://github.com/angus-c/literary.js/blob/master/book/adams/prime.js](https://github.com/angus-c/literary.js/blob/master/book/adams/prime.js)

    ```javascript
    // Here I am, brain the size of a planet, and they ask me to write JavaScript...
    function kevinTheNumberMentioner(_){
      l=[]
      /* mostly harmless --> */ with(l) {
    
        // sorry about all this, my babel fish has a headache today...
        for(ll=!+[]+!![];ll<_+(+!![]);ll++) {
          lll=+!![];
          while(ll%++lll);
          // I've got this terrible pain in all the semicolons down my right hand
    side
          (ll==lll)&&push(ll);
        }
        forEach(alert);
      }
    
      // you're really not going to like this...
      return [!+[]+!+[]+!+[]+!+[]]+[!+[]+!+[]];
    }
    ```
  * 이상의 시가 생각나는 코드 Calvino [https://github.com/angus-c/literary.js/blob/master/book/calvino/sayIt.js](https://github.com/angus-c/literary.js/blob/master/book/calvino/sayIt.js)

    ```javascript
    function sayIt(word) {
      var verse = '';
      //If on a winter's night a programmer
      return chapterOr(word, function chapter1(word) {
        //outside the meaningful logic
        return chapterOr(word, function chapter2(word) {
          //leaning towards deep nesting
          return chapterOr(word, function chapter3(word) {
            //without fear of callback hell
            return chapterOr(word, function chapter4(word) {
              //looks back at the gathering indents
              return chapterOr(word, function chapter5(word) {
                //in a network of functions that enlace
                return chapterOr(word, function chapter6(word) {
                  //in a network of functions that stack
                  return chapterOr(word, function chapter7(word) {
                    //on a carpet of illusions
                    return chapterOr(word, function chapter8(word) {
                      //around an empty core...
                      return chapterOr(word, function chapter9(word) {
                        //What story down there awaits its end?
                        return chapterOr(word, chapter1);
                      });
                    });
                  });
                });
              });
            });
          });
        });
      });
      function chapterOr(word, chapter) {
        word && (verse += (verse && ' ') + word);
        return word ? chapter : verse;
      }
    }
    ```
  * 이 책에서 유일하게 오류로 끝나는 코드라고... Kafka [https://github.com/angus-c/literary.js/blob/master/book/kafka/sayIt.js](https://github.com/angus-c/literary.js/blob/master/book/kafka/sayIt.js)

    ```javascript
    function sayIt(firstWord) {
      var words = [];
      return (function sayIt(word) {
        if (!word) {
          try {
            return sayIt();
          } catch (e) {
            // quitting at last an unsettling recursion,
            // the array was trasformed into a monstrous string
            words = "there's been a hideous bug";
            return words;
          }
        } else {
          words.push(word);
          return sayIt;
        }
      })(firstWord);
    }
    ```
* 하지만 그 어려움에도 불구하고, 책 자체는 작가들에 대한 설명만으로도 재미있게 읽을 수 있긴 하다. 누구나 알듯 코드도 일종의 글쓰기라는 점을 생각하면, 이런 식으로 다양한 스타일로 코드를 작성할 수 있다는 게 일단 재미있기도 하고, 그러면서 새로운 문법을 배울 수도 있고, 무엇보다 더 나은 코드 쓰기를 위한 발판이 될 수 있으니... 한 마디로 좋은 책!
