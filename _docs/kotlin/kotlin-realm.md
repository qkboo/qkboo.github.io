# Realm

## 새로운 Android Application Project

안드로이드 스튜디오에서 새로운 안드로이드 애플리케이션 프로젝트를 만든다.

### 코틀린

액티비티를 Code 메뉴의 "Convert Java File to Kotlin"으로 코틀린 파일로 변경한다.


### build.gralde
프로젝트 그래들 파일에서 repository를 jcenter() 확인한다.

``` xml
buildscript {
    ext.kotlin_version = '1.0.0'
    repositories {
    	// mavenCentral()
        jcenter()		// jcenter는 maven central의 superset
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath "io.realm:realm-gradle-plugin:0.87.4"
    }
}

allprojects {
    repositories {
        jcenter()
    }
}
```
모듈 플러그인 app/build.gralde 파일에서 Realm 플러그인을 적용하고 dependencies를 적용한다.

```xml
apply plugin: 'realm-anroid'

//
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:23.1.1'
    compile 'com.android.support:design:23.1.1'
    compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    kapt "io.realm:realm-annotations:0.87.4"
    kapt "io.realm:realm-annotations-processor:0.87.4"
}
```


### Model

``` kotlin
import io.realm.RealmObject

public open class Cat : RealObject() {
	public open var name: String? = null
}
```

### Write

``` kotlin
private var realm: Realm by Delegates.notNull()
private var realmConfig: RealmConfiguration by Delegates.notNull()

...

realmConfig = RealmConfiguration.Build(this).build()
Realm.deleteRealm(realmConfig)
realm = Realm.getInstance(realConfig)

realm.beginTransaction()
var cat = realm.createObject((Cat::class.java))
cat.name = "뽀뽀"
realm.commitTransaction()

```


### Query

``` kotlin
var cat2 = realm.where((Cat::class.java)).findFirst()
Snackbar.make(fab, "Cat name: ${cat2.name}", Snackbar.LENGTH_LONG).show();
```


## 참조

 - [A Brief introduction to Realm with Kotlin](http://www.slideshare.net/dalinaum/a-brief-introduction-to-realm-with-kotlin)

