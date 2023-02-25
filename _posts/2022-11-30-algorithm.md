---
layout: post
title:  "프로그래머스: 실패율"
categories: algo
comments: true
---





프로그래머스: 숫자 문자열과 영단어

전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 

실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 



---

# CASE 1

~~~js
function solution(N, stages) {
  stages.sort((a,b)=>a-b)
  const failArr = [];
  
  for(let i=1; i<= N; i++){
    failArr.push({
      stage : i,  //스테이지 번호 저장
      users: 0,  //클리어하지 못한 유저의 수
      fail : 0  //실패율
    })
  }
  
  let allUsers = stages.length;
  for(let i=0; i<stages.length; i++){
    // console.log(failArr[stages[i] -1], stages[i])
    if(failArr[ stages[i] - 1] !== undefined){
      failArr [ stages[i] - 1].users++
      
      //현재 스테이지 번호와 다음 스테이지 번호가 다를때
      //현재 스테이지의 정보 참조 끝났을때
      if(stages[i] !== stages[i+1]){
        // console.log(failArr[stages[i]-1]);
        const fail = failArr[stages[i] - 1].users / allUsers;
        // console.log(failArr[stages[i] - 1], allUsers, fail);
        allUsers -= failArr[ stages[i] - 1 ].users;
        
        failArr[ stages[i] - 1].fail = fail;
      }
    }
  }
  
  const answer = failArr.sort((a,b)=>{
    return b.fail - a.fail;
  })
  return answer.map((el)=>{
    return el.stage
  });
}
solution(5, [2, 1, 2, 6, 2, 4, 3, 3])
~~~

---

# CASE 2

~~~js
function solution(N, stages) {
  stages.sort((a,b) => a - b);
  
  let allUsers = stages.length;
  const answer = new Array(N).fill(1).map((num,i) => {
    const stage = num + i;
    // console.log(stage, stages);
    const arr = stages.slice(
      stages.indexOf(stage),
      stages.lastIndexOf(stage) + 1  //앞자리까지만 가져오므로 +1해줘야함
    )
    // console.log(stage, arr)
    const fail = arr.length / allUsers;
    allUsers -= arr.length;
    // console.log(stage, fail);
    return { stage, fail }
  })
  
  answer.sort((a,b)=>{
    return b.fail - a.fail;
  })
  return answer.map((el)=>{
    return el.stage
  });
}
solution(5, [2, 1, 2, 6, 2, 4, 3, 3])
~~~



