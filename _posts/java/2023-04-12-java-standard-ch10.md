---
layout: post
current: post
cover: assets/built/images/java-logo.png
navigation: True
title: Java(10) - 날짜와 시간 & 형식화 / date, time and formatting
date: 2023-04-11 11:31:00
tags:   [java]
class: post-template
subclass: 'post tag-java'
author: rigel9327
---

{% include java-table-of-contents.html %}


1.날짜와 시간

1.1 Calendar 와 Date

▶Calendar는 추상클래스 in java.utill package → 직접 객체 생성불가, 
메서드를 통해 완전히 구현된 클래스의 instance를 얻는다.

Calendar cal = new Calendar(); x
Calendar cal = Calendar.getInstance(); o


Date ↔ Calendar

1. Calendar to Date
   Calendar cal = Calendar.getInstance();

Date d = new Date(cal.getTimeInMillis()); // Date(Long date)

2. Date to Calendar

Date d = new Date();

Calendar cal = Calendar.getInstance();
cal.setTime(d);

// 두 날짜간의 차이를 얻으려면, getTimeInMillis() 천분의 일초 단위로 변환 : 컴퓨터는 시간을 정수로 저장하기 때문

▶날짜 지정하는 법, 월(MONTH)이 0부터 시작한다는 점에 주의
Calendar date1 = Calendar.getInstance();
date1.set(2017, 7, 15);			 // 2017년 8월 15일
//date1.set(Calendar.YEAR, 2017);
//date1.set(Calendar.MONTH, 7);
//date1.set(Calendar.DAY, 15);

▶시간 지정하는 법
Calendar time1 = Calendar.getInstance();
//time1.set(Calendar.HOUR_OF_DAY, 10);
//time1.set(Calendar.MINUTE, 20);
//time1.set(Calendar.SECOND, 30);

▶add()는 특정필드의 값을 증가 또는 감소(다른 필드에 영향o)
Calendar date = Calendar.getInstance();
date.clear();	//모든 필드 초기화
date.set(2020, 7, 31);			//2020년 8월 31일로 설정
date.add(Calendar.DATE, 1);		//날짜(DATE)에 1을 더한다.
date.add(Calendar.MONTH, -8);		//월(MONTH)에서 8을 뺀다.

▶roll()은 특정필드의 값을 증가 또는 감소(다른필드에 영향 x)
date.roll(Calendar.DATE, 1);		//날짜(DATE)에 1을 더한다.
date.roll(Calendar.MONTH, -8);		//월(MONTH)에서 8을 뺀다.

▶clear()는 Calendar 객체의 모든 필드를 초기화
1.Calendar dt = Calendar.getInstance();	//현재시간
//ex) Tue Aug 29 07:13:03 KST 2017
System.out.println(new Date(dt.getTimeInMillis()));
2.dt.clear(); //모든 필드를 초기화
//ex)	Thu Jan 01 00:00:00 KST 1970
System.out.println(new Date(dt.getTimeInMillis()));

▶clear(int field)는 Calendar 객체의 특정필드를 초기화
dt.clear(Calendar.SECOND);		//초를 초기화
dt.clear(Calendar.MINUTE);		//분을 초기화
dt.clear(Calendar.HOUR);		//시간을 초기화
dt.clear(Calendar.HOUR_OF_DAY);	//시간을 초기화

▶get()으로 날짜와 시간 필드 가져오기 -int get(int field)
Calendar cal = Calendar.getInstance();				//현재 날짜와 시간을 세팅됨
int thisYear = cal.get(Calendar.YEAR);				//올해가 몇년인지 알아낸다.
int lastDayOfMonth = cal.getActualMaximum(Calendar.DATE);	//이달의 마지막날

▶Calendar에 정의된 필드
필드명(날짜)									필드명(시간)
YEAR				:년						HOUR			:시간(0~11)
MONTH				:월						HOUR_OF_DAY		:시간(0~23)
WEEK_OF_YEAR		:그 해의 몇번째 주				MINUTE		:분
WEEK_OF_MONTH		:그 달의 몇번째 주				SECOND		:초
DATE				:일						MILLISECOND		:천분의 일초
DAY_OF_MONTH		:그 달의 몇번째 일				ZONE_OFFSET		:GMT 기준 시차(천분의 일초 단위)
DAY_OF_YEAR			:그 해의 몇번째 일				AM_PM			:오전/오후 (0:오전 / 1:오후)
DAY_OF_WEEK			:요일(1~7) ex) 1:일요일
DAY_OF_WEEK_IN_MONTH	:그 달의 몇번째 요일


2. 형식화 클래스

▶날짜를 형식에 맞게 출력하는 클래스 in java.text package

2.1 DecimalFormat

▶숫자 데이터를 정수, 부동소수점, 금액 등의 다양한 형식으로 표현

▶숫자를 형식화 할때 사용 (숫자 → 형식문자열)
double number = 1234567.89;
DecimalFormat df = new DecimalFormat("#.#E0");
String result = df.format(number);	//result="1.2E6"

▶특정 형식의 문자열을 숫자로 변환할 때도 사용(형식문자열 → 숫자)
DecimalFormat df = new DecimalFormat("#,###.##");
Number num = df.parse("1,234,567,89");
double d = num.doubleValue();		//1234567.89

2.2 SimpleDateFormat

▶날짜와 시간을 다양한 형식으로 출력할 수 있게 해준다.
▶특정 형식으로 되어있는 문자열에서 날짜와 시간을 뽑아낼 수도 있다.

▶특정형식으로 되어 있는 문자열에서 날짜와 시간을 뽑아낼 수도 있다.
DateFormat df = new SimpleDateFormat("yyyy년 MM월 dd일");
DateFormat df2 = new SimpleDateFormat("yyyy/MM/dd");
Date d = df.parse("2015년 11월 23일");	//문자열을 Date로 반환
String result = df2.format(d);

2.3 ChoiceFormat

▶특정 범위에 속하는 값을 문자열로 반환

2.4 MessageFormat

▶데이터를 정해진 양식에 맞게 출력할 수 있도록 도와준다.


3. java.time 패키지

▶속한 클래스들은 불변(immutable) → 기존의 객체를 변경하는 대신 항상 변경된 새로운 객체를 반환한다.

3.1 java.time 패키지의 핵심 클래스

▶날짜와 시간을 별도의 클래스로 분리

LocalDate + LocalTime → LocalDateTime
//날짜 + 시간 → 날짜&시간

LocalDateTime + 시간대 → ZonedDateTime

▶Period & Duration
날짜 - 날짜 = Period
시간 - 시간 = Duration

▶객체 생성 - now(), of()
LocalDate date = LocalDate.now();					//2015-11-23
LocalTime time = LocalTime.noew();					//21:54:01.875
LocalDateTime dateTime = LocalDateTime.of(date, time);	//2015-11-23T21:54:01.875
ZonedDateTime dateTimeInKr = ZonedDateTime.now();		//2015-11-23T21:54:01.875+09:00[Asia/Seoul]

LocalDate date = LocalDate.of(2015, 11, 23);	//2015년 11월 23일
LocalTime time = LocalTime.of(23, 59, 59);	//23시 59분 59초

LocalDateTime = dateTime = LocalDateTime.of(datem time);
ZonedDateTime zDateTime = ZonedDateTime.of(dateTime, ZoneId.of("Asia/Seoul"));

▶Temporal & TemporalAmount : interface, 작은 시간의 개념(time, 시분초)과 더 큰 개념의 시간(temporal, 년월일시분초)를 구별
Temporal, TemporalAccessor, TemporalAdjuster → LocalDate, LocalTime, LocalDateTime. ZonedDateTime, Instant ....
TemporalAmount → Period, Duration

▶TemporalUnit & TemporalField
TemporalUnit interface : 날짜와 시간의 단위 정의 → (구현) → ChronoUnit : ENUM

3.2 LocalDate & LocalTime

▶생성 메서드
날짜와 시간 반환 → now()
지정된 날짜와 시간으로 객체 생성 → of()

LocalDate today = LocalDate.now();	//오늘의 날짜
LocalTime new = LocalTime.now();	//현재 시간

LocalDate birthDate = LocalDate.of(1999, 12, 31);	//1999년 12월 31일
LocalTime birthTime = LocalTime.of(23, 59, 59);		//23시 59분 59초

▶get() & getXXX() : 특정 필드의 값 가져오기

▶with(), plus(), minus() : 날짜와 시간에서 특정 필드 값을 변경

▶isAfter(), isBefore(), isEqual() : 날짜와 시간 비교

3.3 Instant

▶EPOCH TIME으로 부터 경과된 시간을 나노초 단위로 표현

▶Instant ↔ Date
Static Date 	from(Instant instant)	//Instant → Date
Instant 		toInstant()			//Date → Instant

3.4 LocalDateTime & ZonedDateTime

▶LocalDate 		+ 	LocalTime 	→ LocalDateTime
▶LocalDateTime 	+ 	시간대 	→ ZonedDateTime

LocalDate + LocalTime → LocalDateTime
LocalDateTime → LocalDate or LocalTime
LocalDateTime → ZonedDateTime

▶ZoneOffset : UTC로부터 얼마만큼 떨어져 있는지를 ZoneOffSet으로 표현

▶OffsetDateTime : ZonedDateTime의 구역을 ZoneId가 아닌 ZoneOffset을 사용하여 표현

▶ZonedDateTime 변환 메서드
LocalDate		toLocalDate()
LocalTime		toLocalTime()
LocalDateTime	toLocalDateTime()
OffsetDateTime	toOffsetDateTime()
long			toEpochSecond()
Instant		toInstant()

3.5 TemporalAdjusters class : 자주 쓰이는 날짜 계산 매서드들을 정의

3.6 Period & Duration

▶between() : date1과 date2의 차이

▶between() & until()
between() is Static method
until() is Instant method

▶of() & with()

▶사칙연산, 비교연산, 기타 메서드

▶toTotalMonths(), toDays(), toHours(), toMinutes() : 다른 단위로 변환

3.7 parsing & formatting