웹크롤링을 하면 jsoup이 어떻게 동작원리가 어떻게 되는 걸까 ?


package com.its.springsecurity.service;

import com.its.springsecurity.dto.*;
import com.its.springsecurity.entity.*;
import com.its.springsecurity.repository.*;
import lombok.RequiredArgsConstructor;
import org.jsoup.Jsoup; 
웹크롤링이라고 하면 파이썬으로 하게된다고 알고 있다 파이썬이 아닌 스프링 환경에서 구축을 했다 . 
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;  
Elements 어노테이션을 사용하면 내가 원하는 정보결과에 접근을 할 수 있게된다 . 
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;




@Service
@RequiredArgsConstructor
public class CrawlingService {
  
    private final CrawlingRepository crawlingRepository;
    private final CrawlingRepository2 crawlingRepository2;

    private final CrawlingRepository3 crawlingRepository3;

    private final CrawlingRepository4 crawlingRepository4;

    private final CrawlingRepository5 crawlingRepository5;

    private final CrawlingRepository6 crawlingRepository6;

    public void craw(CrawlingDTO crawlingDTO) throws IOException {




        String u = "https://www.goal.com/kr/%ED%94%84%EB%A6%AC%EB%AF%B8%EC%96%B4%EB%A6%AC%EA%B7%B8/%EC%88%9C%EC%9C%84/2kwbbcootiqqgmrzs6o5inle5"; // 골스튜디오


        Document doc = Jsoup.connect(u).get();
        Elements elem = doc.getElementsByAttributeValue("class", "main-content");

        Element ele = elem.get(0);
        Elements cons = ele.select("table");
        Elements img = cons.select("td a img"); // 사진

        Elements src = img.select("src");
        Elements names = cons.select("tbody tr"); // 팀 이름
        Elements tNames = names.select("tr ");

        Elements td = tNames.select("td");
        Elements elem2 = doc.getElementsByAttributeValue("class", "widget-match-standings__team--full-name");

        Elements team = td.select("span");


        Elements elements1 = doc.getElementsByAttributeValue("class", "widget-match-standings__matches-played");
        Elements games = elements1.select("td");

        Elements elements2 = doc.getElementsByAttributeValue("class", "widget-match-standings__matches-won");
        Elements wins = elements2.select("td");


        Elements elements3 = doc.getElementsByAttributeValue("class", "widget-match-standings__matches-drawn");
        Elements draws = elements3.select("td");

        Elements elements4 = doc.getElementsByAttributeValue("class", "widget-match-standings__matches-lost");
        Elements lost = elements4.select("td");

        Elements elements5 = doc.getElementsByAttributeValue("class", "widget-match-standings__goals-diff");
        Elements diff = elements5.select("td");

        Elements elements6 = doc.getElementsByAttributeValue("class", "widget-match-standings__pts");
        Elements points = elements6.select("td");

        Elements elements7 = doc.getElementsByAttributeValue("class", "widget-match-standings__goals-for");
        Elements goals = elements7.select("td");

        Elements elements8 = doc.getElementsByAttributeValue("class", "widget-match-standings__goals-against");
        Elements goalLost = elements8.select("td");


        CrawlingDTO crawlingDTO1 = new CrawlingDTO();
        CrawlingEntity entity = new CrawlingEntity();


        List<CrawlingEntity> crawlingEntityList = new ArrayList<>();
        for (int i = 0; i < elem2.size(); i++) {     // 반복문을 통해 저장처리
            CrawlingDTO crawDTO = new CrawlingDTO();
            crawDTO.setTeam(elem2.get(i).text());
            crawDTO.setGames(games.get(i).text());
            crawDTO.setWin(wins.get(i).text());
            crawDTO.setDraw(draws.get(i).text());
            crawDTO.setLose(lost.get(i).text());
            crawDTO.setPlus(goals.get(i).text());
            crawDTO.setMinus(goalLost.get(i).text());
            crawDTO.setDiff(diff.get(i).text());
            crawDTO.setPoint(points.get(i).text());


            CrawlingEntity existingEntity = crawlingRepository.findByTeam(crawDTO.getTeam());
            if (existingEntity != null) {
                existingEntity.setGames(crawDTO.getGames());
                existingEntity.setWin(crawDTO.getWin());
                existingEntity.setDraw(crawDTO.getDraw());
                existingEntity.setLose(crawDTO.getLose());
                existingEntity.setPlus(crawDTO.getPlus());
                existingEntity.setMinus(crawDTO.getMinus());
                existingEntity.setDiff(crawDTO.getDiff());
                existingEntity.setPoint(crawDTO.getPoint());
                crawlingRepository.save(existingEntity);
            } else {
                // Save a new entry to the database
                crawlingRepository.save(CrawlingEntity.toSaveEntity(crawDTO));
            }
        }
        
마무리 : jsoup에 대한 정보가 많이 없어서 chatgpt에 질문을 해서 얻어온 답이다 . 
프로젝트를 하면서 시간에 쫓겨 jsoup을 파악을 못했엇다 이번기회에 알아가는 기회였던 것 같다 . 
        
        
Jsoup은 HTML 문서를 구문 분석하고, 내용을 조작하고, 데이터를 추출할 수 있는 Java 라이브러리입니다. CSS와 같은 선택기 또는 XPath 표현식을 기반으로 HTML 문서에서 특정 요소를 선택하고 추출할 수 있는 일련의 방법을 제공함으로써 작동합니다.

Jsoup으로 HTML 문서를 구문 분석하면, 라이브러리는 페이지의 전체 HTML 구조를 나타내는 Document 오브젝트를 작성합니다. 그런 다음 제공된 선택기 메서드를 사용하여 이 개체를 쿼리하여 특정 요소 또는 속성을 추출할 수 있습니다.

예를 들어 getElementById() 메서드를 사용하여 특정 ID의 요소를 검색하거나 getElementsByClass() 메서드를 사용하여 특정 CSS 클래스의 모든 요소를 검색할 수 있습니다.

원하는 요소를 선택한 후에는 속성이나 텍스트 내용을 변경하여 요소를 조작할 수 있습니다. 또한 Jsoup을 사용하여 새 요소를 삽입하거나 문서의 구조를 수정할 수 있습니다.

마지막으로 HTML 문서 작업을 마치면 Jsoup을 사용하여 수정된 HTML을 문자열로 출력하거나 파일에 저장할 수 있습니다.

전반적으로 Jsoup은 Java 환경에서 HTML 컨텐츠로 작업할 수 있는 강력하고 유연한 방법을 제공하므로 웹 스크래핑, 데이터 추출 및 HTML 문서 작업과 관련된 기타 작업에 널리 사용됩니다.




