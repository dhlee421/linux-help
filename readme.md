잘 정리된 사이트
https://kabkee.github.io/mysql/mysql-full-text-search/

설정 
$ sduo vi /etc/mysql/mysql.conf.d/mysqld.cnf
[mysqld]
ft_min_word_len = 2
innodb_ft_min_token_size = 2

최소 인덱싱 글자수 확인
show variables like 'ft_min%'


 참조
 http://www.hackingwithphp.com/9/3/18/advanced-text-searching-using-full-text-indexes
 https://www.w3resource.com/mysql/mysql-full-text-search-functions.php
 
FULLTEXT 검색 방식
자연어 검색(natural search)
검색 문자열을 단어 단위로 분리한 후, 해당 단어 중 하나라도 포함되는 행을 찾는다.
불린 모드 검색(boolean mode search)
검색 문자열을 단어 단위로 분리한 후, 해당 단어가 포함되는 행을 찾는 규칙을 추가적으로 적용하여 해당 규칙에 매칭되는 행을 찾는다.
쿼리 확장 검색(query extension search)
2단계에 걸쳐서 검색을 수행한다. 첫 단계에서는 자연어 검색을 수행한 후, 첫 번째 검색의 결과에 매칭된 행을 기반으로 검색 문자열을 재구성하여 두 번째 검색을 수행한다. 이는 1단계 검색에서 사용한 단어와 연관성이 있는 단어가 1단계 검색에 매칭된 결과에 나타난다는 가정을 전제로 한다.

create table posts(
    id bigint(100) NOT NULL AUTO_INCREMENT,
    gtitle text NOT NULL,
    gdesc text NOT NULL,
    ....
    PRIMARY KEY (id),
    FULLTEXT KEY gtitle (gtitle)
) ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

alter table posts add FULLTEXT(gtitle);


ALTER TABLE `기존 테이블명` 
ADD FULLTEXT INDEX `인덱스명` (`컬럼명`) VISIBLE;

-- Ex1.
ALTER TABLE `bbs` 
ADD FULLTEXT INDEX `subject` (`subject`) VISIBLE;

-- Ex2.
ALTER TABLE `기존 테이블명` 
ADD FULLTEXT INDEX `subject_tag` (`subject, tag`) VISIBLE;


SELECT * FROM bbs WHERE MATCH(subject) AGAINST("코로나*" in boolean mode)
-- or
SELECT * FROM bbs WHERE MATCH(subject,tag) AGAINST("+코로나* +언니*" in boolean mode)


