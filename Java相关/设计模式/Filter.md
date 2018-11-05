# 结构模式-过滤器模式（Filter）
过滤器模式（Filter）也称标准模式（Criteria），使用不同的标准来过滤一组对象，通过逻辑运算以解耦的方式把它们连接起来。
#### 实现
我们将创建一个 Person 对象、Criteria 接口和实现了该接口的实体类，来过滤 Person 对象的列表。CriteriaPatternDemo，我们的演示类使用 Criteria 对象，基于各种标准和它们的结合来过滤 Person 对象的列表。
![](../../resources/filter.jpg)

```
Person：实体类
Criteria：过滤器接口
MaleCriteria：男过滤器类
FemaleCriteria：女过滤器类
SingleCriteria：单身过滤器类
AndCriteria：与过滤器组合
OrCriteria：或过滤器组合
```
Person类

```java
@Data
public class Person {
    private String name;
    private String gender;
    private String maritalStatus;

    public Person(String name, String gender, String maritalStatus) {
        this.name = name;
        this.gender = gender;
        this.maritalStatus = maritalStatus;
    }
}
```
Criteria接口

```java
public interface Criteria {
    List<Person> meetCriteria(List<Person> people);
}
```
MaleCriteria类

```java
public class MaleCriteria implements Criteria {
    @Override
    public List<Person> meetCriteria(List<Person> people) {
        List<Person> malePeople = new ArrayList<>();
        for (Person p : people) {
            if ("Male".equalsIgnoreCase(p.getGender())) {
                malePeople.add(p);
            }
        }
        return malePeople;
    }
}
```
FemaleCriteria类

```java
public class FemaleCriteria implements Criteria {
    @Override
    public List<Person> meetCriteria(List<Person> people) {
        List<Person> femalePeople = new ArrayList<>();
        for (Person p : people) {
            if ("Female".equalsIgnoreCase(p.getGender())) {
                femalePeople.add(p);
            }
        }
        return femalePeople;
    }
}
```
SingleCriteria类

```java
public class SingleCriteria implements Criteria {
    @Override
    public List<Person> meetCriteria(List<Person> people) {
        List<Person> singlePeople = new ArrayList<>();
        for (Person p : people) {
            if ("Single".equalsIgnoreCase(p.getMaritalStatus())) {
                singlePeople.add(p);
            }
        }
        return singlePeople;
    }
}
```
AndCriteria类

```java
public class AndCriteria implements Criteria {
    private Criteria mCriteria;
    private Criteria mOtherCriteria;

    public AndCriteria(Criteria mCriteria, Criteria mOtherCriteria) {
        this.mCriteria = mCriteria;
        this.mOtherCriteria = mOtherCriteria;
    }

    @Override
    public List<Person> meetCriteria(List<Person> people) {
        List<Person> firstCriteriaPeople = mCriteria.meetCriteria(people);
        List<Person> secondCriteriaPeople = mOtherCriteria.meetCriteria(firstCriteriaPeople);
        return secondCriteriaPeople;
    }
}

```
OrCriteria类

```java
public class OrCriteria implements Criteria {
    private Criteria mCriteria;
    private Criteria mOtherCriteria;

    public OrCriteria(Criteria mCriteria, Criteria mOtherCriteria) {
        this.mCriteria = mCriteria;
        this.mOtherCriteria = mOtherCriteria;
    }

    @Override
    public List<Person> meetCriteria(List<Person> people) {
        List<Person> firstCriteriaPeople = mCriteria.meetCriteria(people);
        List<Person> secondCriteriaPeople = mOtherCriteria.meetCriteria(firstCriteriaPeople);
        for (Person p : secondCriteriaPeople) {
            if (!firstCriteriaPeople.contains(p)) {
                firstCriteriaPeople.add(p);
            }
        }
        return firstCriteriaPeople;
    }
}

```

过滤器的使用

```java
List<Person> people = new ArrayList<>();
people.add(new Person("Robert", "Male", "Single".toString()));
people.add(new Person("John", "Male", "Married".toString()));
people.add(new Person("Laura", "Female", "Married".toString()));
people.add(new Person("Diana", "Female", "Single".toString()));
people.add(new Person("Mike", "Male", "Single".toString()));
people.add(new Person("Bobby", "Male", "Single".toString()));

Criteria male = new MaleCriteria();
Criteria female = new FemaleCriteria();
Criteria single = new SingleCriteria();
Criteria maleAndSingle = new AndCriteria(male, single);
Criteria maleOrSingle = new OrCriteria(male, single);

List<Person> malePeople = male.meetCriteria(people);
List<Person> femalePeople = female.meetCriteria(people);
List<Person> singlePeople = single.meetCriteria(people);
List<Person> maleAndSinglePeople = maleAndSingle.meetCriteria(people);
List<Person> maleOrSinglePeople = maleOrSingle.meetCriteria(people);
```

