---
layout: post
title: "[Liquibase] Liquibase 기초"
published: true
comments: true
---

# Liquibase 기초

#### 리퀴베이스란?
- DB 형상관리 툴

---

#### 설정 (Spring Boot, PostgreSQL 사용)
- build.gradle
![2022-12-01-Liquibase-01.png](https://hoonk212.github.io/assets/images/2022-12-04-Liquibase-01.png)
- application.yaml (yaml 기준)
![2022-12-01-Liquibase-02.png](https://hoonk212.github.io/assets/images/2022-12-04-Liquibase-02.png)

---

#### 구성
![2022-12-01-Liquibase-03.png](https://hoonk212.github.io/assets/images/2022-12-04-Liquibase-03.png)

---

### 예시
- changelog-master.xml

  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <databaseChangeLog
          xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
              http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.8.xsd">

      <!-- account -->
      <include file="classpath:/db/changelog/changelog-v1.0.xml"/>
      <!-- board -->
      <include file="classpath:/db/changelog/changelog-v1.1.xml"/>
      <!-- recommend -->
      <include file="classpath:/db/changelog/changelog-v1.2.xml"/>

  </databaseChangeLog>
  ```

- changelog-v1.0.xml (계정)

  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <databaseChangeLog
          xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
              http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.8.xsd">

      <!-- create account table -->
      <changeSet id="1" author="HoonK">
          <createTable tableName="account">
              <column name="idx" type="int" autoIncrement="true">
                  <!-- primary key : idx -->
                  <constraints primaryKeyName="account_pkey" primaryKey="true"
                              nullable="false"/>
              </column>
              <column name="email" type="varchar(100)"/>
              <column name="passwd" type="varchar(100)"/>
          </createTable>

          <!-- sequence -->
          <createSequence sequenceName="account_idx_seq"/>
      </changeSet>

      <!-- create idx_account_email index -->
      <changeSet id="2" author="HoonK">
          <createIndex indexName="idx_account_email" tableName="account">
              <column name="email" type="varchar(100)"/>
          </createIndex>
      </changeSet>

  </databaseChangeLog>
  ```

- changelog-v1.1.xml (게시판)

  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <databaseChangeLog
          xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
              http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.8.xsd">

      <!-- create account table -->
      <changeSet id="1" author="HoonK">
          <createTable tableName="account">
              <column name="idx" type="int" autoIncrement="true">
                  <!-- primary key : idx -->
                  <constraints primaryKeyName="account_pkey" primaryKey="true"
                              nullable="false"/>
              </column>
              <column name="email" type="varchar(100)"/>
              <column name="passwd" type="varchar(100)"/>
          </createTable>

          <!-- sequence -->
          <createSequence sequenceName="account_idx_seq"/>
      </changeSet>

      <!-- create idx_account_email index -->
      <changeSet id="2" author="HoonK">
          <createIndex indexName="idx_account_email" tableName="account">
              <column name="email" type="varchar(100)"/>
          </createIndex>
      </changeSet>

  </databaseChangeLog>
  ```

- changelog-v1.2.xml (추천)

  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <databaseChangeLog
          xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
              http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.8.xsd">

      <!-- create account table -->
      <changeSet id="1" author="HoonK">
          <createTable tableName="account">
              <column name="idx" type="int" autoIncrement="true">
                  <!-- primary key : idx -->
                  <constraints primaryKeyName="account_pkey" primaryKey="true"
                              nullable="false"/>
              </column>
              <column name="email" type="varchar(100)"/>
              <column name="passwd" type="varchar(100)"/>
          </createTable>

          <!-- sequence -->
          <createSequence sequenceName="account_idx_seq"/>
      </changeSet>

      <!-- create idx_account_email index -->
      <changeSet id="2" author="HoonK">
          <createIndex indexName="idx_account_email" tableName="account">
              <column name="email" type="varchar(100)"/>
          </createIndex>
      </changeSet>

  </databaseChangeLog>
  ```

---

### 예시 (yaml)
- changelog-master.xml

  ```
    databaseChangeLog:
    -  includeAll:
        path: liquibase/changelogs/
  ```

- v1.0-accout.yaml (계정)

  ```
    databaseChangeLog:
    # create account table
    -  changeSet:
        id:  1
        author:  HoonK
        changes:
        -  createTable:
            tableName:  account
            columns:
            -  column:
                name:  idx
                type:  int
                autoIncrement:  true
                # primary key : idx
                constraints:
                    primaryKeyName: account_pkey
                    primaryKey:  true
                    nullable:  false
            -  column:
                name:  email
                type:  varchar(100)
            -  column:
                name:  passwd
                type:  varchar(100)
        # sequence
        - createSequence:
            sequenceName: account_idx_seq

    # create idx_account_email index
    - changeSet:
        id:  2
        author:  HoonK
        changes:
        - createIndex:
            indexName: idx_account_email
            tableName: account
            columns:
            - column:
                name: email
                type: varchar(100)
  ```

- v1.1-board.yaml (게시판)

  ```
    databaseChangeLog:
    # create board table
    -  changeSet:
        id:  1
        author:  HoonK
        changes:
        -  createTable:
            tableName:  board
            columns:
            -  column:
                name:  idx
                type:  int
                autoIncrement:  true
                # primary key : idx
                constraints:
                    primaryKeyName: board_pkey
                    primaryKey:  true
                    nullable:  false
            -  column:
                name:  title
                type:  varchar(100)
            -  column:
                name:  contents
                type:  varchar(100)
            -  column:
                name:  account_idx
                type:  int
                # foreign key : account(idx)
                constraints:
                    foreignKeyName: board_account_idx_fkey
                    referencedTableName:  account
                    referencedColumnNames:  idx
        # sequence
        - createSequence:
            sequenceName: board_idx_seq

    # create idx_board_title index
    - changeSet:
        id:  2
        author:  HoonK
        changes:
        - createIndex:
            indexName: idx_board_title
            tableName: board
            columns:
            - column:
                name: title
                type: varchar(100)
  ```

- v1.2-recommend.yaml (추천)

  ```
    databaseChangeLog:
    # create recommend table
    -  changeSet:
        id:  1
        author:  HoonK
        changes:
        -  createTable:
            tableName:  recommend
            columns:
            -  column:
                name:  idx
                type:  int
                autoIncrement:  true
                # primary key : idx
                constraints:
                    primaryKeyName: recommend_pkey
                    primaryKey:  true
                    nullable:  false
            -  column:
                name:  account_idx
                type:  int
                # foreign key : account(idx)
                constraints:
                    foreignKeyName: recommend_account_idx_fkey
                    referencedTableName:  account
                    referencedColumnNames:  idx
            -  column:
                name:  board_idx
                type:  int
                # foreign key : account(idx)
                constraints:
                    foreignKeyName: recommend_board_idx_fkey
                    referencedTableName:  board
                    referencedColumnNames:  idx
        # sequence
        - createSequence:
            sequenceName: recommend_idx_seq

    # add recommend_account_idx_board_idx_key constraint
    - changeSet:
        id:  2
        author:  HoonK
        changes:
        - addUniqueConstraint:
            constraintName: recommend_account_idx_board_idx_key
            tableName: recommend
            columnNames: account_idx, board_idx
  ```

---

#### databasechangelog
- 실질적인 형상관리 테이블 (캡처 : DBeaver)
![2022-12-01-Liquibase-04.png](https://hoonk212.github.io/assets/images/2022-12-04-Liquibase-04.png)
![2022-12-01-Liquibase-05.png](https://hoonk212.github.io/assets/images/2022-12-04-Liquibase-05.png)

---

#### databasechangeloglock
- 에러가 발생할 경우, Liquibase의 table lock 관리를 위한 테이블
