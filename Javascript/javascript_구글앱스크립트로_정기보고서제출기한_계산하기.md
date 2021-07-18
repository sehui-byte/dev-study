# javascript 구글 앱 스크립트로 정기보고서 제출기한 계산하기

## 정기보고서(사업보고서, 분기보고서란)

- 정기보고서는 기업의 사업내용과 재무상황 등을 일반투자자가 확인할 수 있는 좋은 방법이다.
- 모든 주식시장에 상장한 상장기업은 정기보고서의 제출 의무를 가지고 있으며 
- 미제출시 관리종목 지정, 상장폐지의 요건이 된다.

## 정기보고서의 제출기한

- 1, 2, 3분기의 분기보고서는 결산월 기준 해당 분기적용일로부터 45일 이내
- 4분기 보고서인 사업보고서는 결산월 기준 해당 분기적용일로부터 90일 이내
- 1, 2, 3분기의 분기보고서는 연결기준으로 최초 제출일 경우 최초 사업연도와 그 다음 사업 연도에 한하여
  최종 제출기일로부터 최대 15일까지 연장이 가능하다. (제출기한은 = 총 60일 이내)
- 최종 제출기일이 주말일 경우 : 해당 주말을 건너 뛰고 다음 월요일로 적용

## 함수 소개
- 1.reportDate(year, month, quarter) : 년도, 월, 해당 분기를 보내주면 정기보고서 제출기일을 반환하는 함수
- 2.monthAddRemainder(sum_month) : 12월을 초과한 값으로 다음 년도의 월을 반환하는 함수
> ex) 11월 +5 일경우 4월 반환   
   
- 3.monthAddQuotient(year, sum_month) : 12월을 초과한 값으로 다음 년도를 반환하는 함수
> ex) 2021 12월 +5 = 2022 반환   
    
- 4.sumMonth(month, wantMath) : 해당 분기에 해당하는 월 계산하기 (결산월 + 분기 * 3 값을 반환하는 함수)
- 5.week(dayNum) : 주말 여부에 따라 일요일일 경우 1, 토요일일 경우 2를 반환하는 함수, 나머지는 0일
- 6.date_zero_num_to_str(year, month, day) : 월, 일에 0을 붙여서 yyyy/mm/dd 형식으로 변환해주는 함수
> ex) 1월 -> 01월 / 11월 -> 11월   
    
- 7.day_math(year, month, taget_day, want_day_num, month_last_day) : 분기보고서 제출일에서 최장 연장기간인 +15일을 계산하는 함수
- 8.closing_to_month(year, month, quarter) : 정기보고서 제출일을 계산하는 함수

## 코드 내용

### 1.reportDate()   

```javascript
//1.reportDate(year, month, quarter) : 년도, 월, 해당 분기를 보내주면 정기보고서 제출기일을 반환하는 함수

//결산월 매칭
/**
 * 년월,분기를 이용하여 사업보고서 제출기한을 계산한다. 
 * 함수사용법 => (년도, 월, 몇분기?, 반환받고싶은 결과 (디폴트=method)입력 안해도 됨 )
 * @param  {year} int 년도 4자리 ex)2021
 * @param  {month} int 기준 결산월
 * @param  {quarter} int 몇분기인지? ex) 1,2,3,4
 * @return 결산월의 범위 
 * @customfunction
 */
function reportDate(year, month, quarter){

  if(quarter > 4){
    return "분기가 4보다 클 수는 없읍디다."
  }
  //숫자로 형변환하기
  year = typeof(year) != "number" ? parseInt(year): year
  month = typeof(month) != "number" ? parseInt(month): month
  quarter = typeof(quarter) != "number" ? parseInt(quarter): quarter
  
  let sum_month = 0
  if(quarter < 4 && quarter > 0){
    sum_month = sumMonth(month,(quarter*3)); // 해당 분기에 해당하는 월 구하기
    month = monthAddRemainder(sum_month); //나머지구하기 (초과한 월에서 12월 기준 계산)
  }

  return closing_to_month(year, month, quarter);

}
```
### 2.monthAddRemainder()   

```javascript
//2.monthAddQuotient(year, sum_month) : 12월을 초과한 값으로 다음 년도를 반환하는 함수

//나머지구하기 (초과한 월에서 12월 기준 계산)
function monthAddRemainder(sum_month){
  if(sum_month >= 13){
    return parseInt(sum_month % 13)+1
  }else{
    return sum_month
  }
}
```

### 3.monthAddQuotient()   

```javascript
//3.monthAddQuotient(year, sum_month) : 12월을 초과한 값으로 다음 년도를 반환하는 함수

//몫구하기 (초과한 월만큼의 년도 구하기)
function monthAddQuotient(year, sum_month){
  if(sum_month >= 13){
    return parseInt(year + (sum_month / 13))
  }else{
    return year
  }
}
```
### 4.sumMonth()   

```javascript
//4.sumMonth(month, wantMath) : 해당 분기에 해당하는 월 계산하기 (결산월 + 분기 * 3 값을 반환하는 함수)

//결산월 + 분기*3
function sumMonth(month, wantMath){
  month = month + wantMath
  return month
}
```
### 5.week()

```javascript
//5.week(dayNum) : 주말 여부에 따라 일요일일 경우 1, 토요일일 경우 2를 반환하는 함수, 나머지는 0일

//정기보고서 제출기한에서 마지막 제출 기일이 주말일 경우 해당 주말의 다음 월요일이 제출일이 된다.
//주말 여부에 따라 월요일만큼 값을 + 시키기 위해 주말의 숫자 값을 넘겨주는 함수
function week (dayNum){
  //               일 월 화 수  목 금  토 
  let week_list = [1, 0, 0, 0, 0, 0, 2];

  return week_list[dayNum];
}
```

### 6.date_zero_num_to_str()   

```javascript
6.date_zero_num_to_str(year, month, day) : 월, 일에 0을 붙여서 yyyy/mm/dd 형식으로 변환해주는 함수

//각각의 값을 문자열로 치환하여 0을 붙이고 연결시킨 문자열을 반환한다.
function date_zero_num_to_str(year, month, day){
  year = (year + "").length == 1 ? "0" + year : year;
  month = (month + "").length == 1 ? "0" + month : month;
  day = (day + "").length == 1 ? "0" + day : day;
  
  //return year + "/" + month + "/" + day;
  return month + "/" + day;
}
```

### 7.day_math()   

```javascript
7.day_math(year, month, taget_day, want_day_num, month_last_day) : 분기보고서 제출일에서 최장 연장기간인 +15일을 계산하는 함수

//최소 제출일에서 최장 연장기간인 15일을 더한 값을 반환한다.
function day_math(year, month, taget_day, want_day_num, month_last_day){
  taget_day = taget_day + want_day_num;
  if(taget_day > month_last_day){
    month = month + 1;
    taget_day = taget_day - month_last_day;
  }
  if(want_day_num == 0){
    return "";
  }
  //return "~" + year + "/" + month + "/" + taget_day;
  return date_zero_num_to_str(year, month, taget_day);
}
```

### 8.closing_to_month()

```javascript
- 8.closing_to_month(year, month, quarter) : 정기보고서 제출일을 계산하는 함수

//해당 분기의 사업보고서 최소 제출의 월 구하기
//보고서의 계산일자는 1분기일 경우 결산월+3에서 해당 월의 말일의 다음날부터 제출기한이 계산된다.
//따라서 기본 month 매개변수에 +1을 해준다.
function closing_to_month(year, month, quarter){
  month = month + 1; //결산월+3에서 해당 월의 말일의 다음날부터 제출기한이 계산된다.
                      //결산월이 1월이고 1분기의 보고서 제출월이 4월이라면 정기보고서제출일 계산은 5월 1일부터 시작된다.

  let date = new Date(year, month, 0); // 해당 년도의 해당 월의 Date 객체 선언
  let last_day = date.getDate();  // 해당 년도, 해당 월의 마지막 요일 가져오기
  let default_day = quarter !=4 ? 45 : 90; //정기보고서 제출일은 해당 결산월에서 4분기를 제외한 나머지 1,2,3 분기는 45일의 제출 기한을 준다.
                                          //4분기에 제출하게 될 1년동안의 사업보고서는 90일의 제출 기한을 준다.
  
  for( ; default_day > last_day ; ){
      default_day = default_day - last_day;
      month = month + 1;
      month = monthAddRemainder(month); //나머지구하기 (초과한 월에서 12월 기준 계산)
      date = new Date(year, month, 0);
      last_day = date.getDate();
  }
  last_day = default_day

  
  
  day_number = week(new Date(year+'-'+month+'-'+last_day).getDay());
  last_day = last_day + day_number;
  month_last_day = date.getDate();
  if(last_day > month_last_day){
    month = month + 1;
    last_day = last_day - month_last_day;
  }

  //return date_zero_num_to_str(year, month, last_day) + (quarter != 4 ? "~": "") + day_math(year, month, last_day, (quarter !=4 ? 15 : 0), month_last_day);

  return date_zero_num_to_str(year, month, last_day) + 
        (quarter != 4 ? "(": "") + 
            day_math(year, month, last_day, (quarter !=4 ? 15 : 0), month_last_day) + 
        (quarter != 4 ? ")": "");
}
```
### 결과
<img src="https://github.com/joohyoungkim19940805/imgRepository/blob/98a82cb256a76955bb4408de3d37551356a01002/testFile/zldqkesp.JPG?raw=true" width="70%" height="30%" title="px(픽셀) 크기 설정" alt="RubberDuck"></img>

- 어떻게 17일 + 15일을 31일로 계산했지? 킹받네 정말

