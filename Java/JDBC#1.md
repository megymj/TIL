# JDBC 사용 예제 #1

 MySQL database table 안에 들어 있는 500개의 디렉토리 이름 중, 특정 조건을 만족하는 디렉토리의 개수는 113개이다. 113개에 해당하는 디렉토리 이름을 mysql에서 읽어와서 ArrayList에 담은 뒤, 해당 ArrayList에 담겨 있지 않은 나머지 377개의 디렉토리를 제거하는 것이 목적이다.

```sql
# 해당 쿼리는 java code에서 사용한 쿼리(특정 조건)
SELECT dir_name FROM test
WHERE unique_count = 1;
```



<br>



## 1. Intellij > New gradle project 생성

> 1-1. 생성 후 dependency에 mysql-connector 추가

```java
// build.gradle 
...
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'

    // https://mvnrepository.com/artifact/mysql/mysql-connector-java
    implementation group: 'mysql', name: 'mysql-connector-java', version: '8.0.28'

}
```



> 1-2. 코드 실행 시 finished with non-zero exit value 1 오류 해결을 위한 Intellij 설정 변경
>
> > **Preference -> Build, Execution, Deployment -> Build Tools -> Gradle**에서, **Build and run using, Run tests using** 부분을 기존 **Gradle** 로 되어있었는데 **IntelliJ IDEA**로 변경.
> >
> > 아래 **Gradle JVM** 역시 프로젝트에 맞는 **java version**으로 변경.





<br>



## 2. JDBC 코드

> 원래 목적은 resources 하위에 있는 500개의 directory 중 조건에 해당하지 않는 것을 삭제하는 것이었으나, 목적대로 코드를 작성하지 못하였다. 대신 조건에 해당하는 코드를 save 디렉토리로 옮기도록 코드를 작성하였다.
>
> -> 2-1 에서 해결함

```java
import java.io.File;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class DBConnect {

    public static void main(String[] args) throws SQLException {

        Connection con;
        Statement stmt;
        ResultSet rs;

        String url = "jdbc:mysql://localhost/[db name]";
        String user = " ";
        String password = " ";

        con = DriverManager.getConnection(url, user, password);
        System.out.println("db connection is succeed");

        stmt = con.createStatement();
        rs = stmt.executeQuery("select dir_name from test where unique_count = 1");

        List<String> dbDirList = new ArrayList<>();
        while(rs.next()) {
            dbDirList.add(rs.getString(1));
        }
        System.out.println(dbDirList.size());


        rs.close();
        stmt.close();
        con.close();
        System.out.println("db connection is done");

        String path = "/Users/moojun/Desktop/Directory_Read/src/main/resources";
        File folder = new File(path);

        List<String> fileList = new ArrayList<>();

        int isFold = 0;
        for (File info : new File(path).listFiles()) {
            if (info.isDirectory() && dbDirList.contains(info.getName())) {
                //System.out.println(info.getName());
                isFold++;
                File moveFile = new File("/Users/moojun/Desktop/Directory_Read/src/main/save";, info.getName());
                info.renameTo(moveFile);
            }
        }
        System.out.println("이동된 dir의 개수는: " + isFold + " 개");

    }
}

```





<br>



### 2-1. JDBC 코드

> 정상적으로 기존의 디렉토리 중, 조건을 만족하지 않는 디렉토리를 제거한 코드.
>
> **Directory 는 내부에 파일이 하나라도 남아있으면 File.delete() 함수가 작동하지 않는다.**
>
> File.length() 는 파일의 byte단위 크기를 return 한다.
>
> 현재 resources 디렉토리 내의 구조는 아래와 같다.
>
> resource/change001/old/ex.java
>
> resource/change001/new/ex.java
>
> ​      

```java
import java.io.File;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class DBConnect {

    private static int remainCount = 0;
    private static List<String> dbDirList;

    public static void main(String[] args) throws SQLException {

        Connection con;
        Statement stmt;
        ResultSet rs;

        String url = "jdbc:mysql://localhost/[db name]";
        String user = " ";
        String password = " ";

        con = DriverManager.getConnection(url, user, password);
        System.out.println("db connection is succeed");

        stmt = con.createStatement();
        rs = stmt.executeQuery("select dir_name from test where unique_count = 1");

        dbDirList = new ArrayList<>();
        while(rs.next()) {
            dbDirList.add(rs.getString(1));
//            System.out.println(rs.getString(1));
        }
        System.out.println(dbDirList.size());

        rs.close();
        stmt.close();
        con.close();
        System.out.println("db connection end");

        String path = "/Users/moojun/Desktop/Directory_Read/src/main/resources";

        // Directory 는 하위에 파일이 하나라도 남아있으면 File.delete() 함수가 작동하지 않는다.
        // File.length() 는 파일의 byte 단위 크기를 return 한다.
        deleteFolder(path);
        System.out.println("전체 500개 중 남은 디렉토리의 개수는: " + remainCount + "개");

    }

    // 재귀함수를 호출하여 밑에 있는 폴더부터 차례대로 제거한다.
    // 디렉토리 안에 디렉토리 ... 이런 구조가 반복될 때 사용한다.
    public static void deleteFolder(String path) {

        File folder = new File(path);

        List<String> test = new ArrayList<>();

        try {
            if (folder.isDirectory()) { // 만약 최상위 folder 인 resources 가 directory 이면,
                File [] folderArr = folder.listFiles();

                for (int i = 0; i < folderArr.length; i++) {
                    
                    // 조건 설정
                    if (dbDirList.contains(folderArr[i].getName())) {
                        remainCount++;
                        continue;
                    }

                    if (folderArr[i].isFile()) {
                        boolean b1 = folderArr[i].delete();
                    } else {
                        deleteFolder(folderArr[i].getPath());   // 재귀 호출
                    }
                    boolean b2 = folderArr[i].delete();
                }
                //boolean b3 = folder.delete(); // 최상위 폴더 제거
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

