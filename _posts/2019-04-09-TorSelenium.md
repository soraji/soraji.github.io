---
layout: post
title: "[ Java ] TOR 브라우저 사용기 with 셀레니움 (tor selenium)"
categories: back
comments: true
---

Tor는 ip를 우회할때 사용하는 브라우저이다.

Tor브라우저를 사용하니 검색창에 내ip검색해도 완전 다른 아이피가 나온다.

크롤링좀했다고 ip막아버린곳에 Tor로 작업중이다.

공공기관은 데이터를 오픈하는게 원래는 정상인데, 이걸 막아놓다니... 결국에는 내가 고생임

```java
public class consolelog{

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		System.setProperty("webdriver.gecko.driver", "geckodriver.exe가 있는 디렉토리 경로");
		File torProfileDir = new File( "C:/Users/sora/Desktop/Tor Browser/Browser/TorBrowser/Data/Browser/profile.default");
		FirefoxBinary binary = new FirefoxBinary(new File( "C:/Users/sora/Desktop/Tor Browser/Browser/firefox.exe"));
		FirefoxProfile torProfile = new FirefoxProfile(torProfileDir);
		torProfile.setPreference("webdriver.load.strategy", "unstable");



		try {
			binary.startProfile(torProfile, torProfileDir, "");

		}catch (IOException e) {
			e.printStackTrace();
		}

		FirefoxProfile profile = new FirefoxProfile();
		profile.setPreference("network.proxy.type", 1);
		profile.setPreference("network.proxy.socks", "127.0.0.1");
		profile.setPreference("network.proxy.socks_port", 9150);
		profile.setPreference("permissions.default.image", 2);
		profile.setPreference("permissions.default.stylesheet", 2);
		profile.setPreference("javascript.enable", false);



		FirefoxOptions options = new FirefoxOptions();
		options.setProfile(profile);
		options.addArguments("-headless");
//		options.setProfile(torProfile);
//		options.setBinary(binary);

		try{
			Date date = new Date();
			SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");
			String today = sdf.format(date);

			Calendar cal = Calendar.getInstance();
			cal.add(cal.DATE, -7);
			Date predate = cal.getTime();
			String predate1 = sdf.format(predate);

			FirefoxDriver driver= new FirefoxDriver(options);
			driver.get("https://check.torproject.org/");
			driver.getPageSource();

			//토르브라우저를 사용하는지 확인
			if(driver.getPageSource().indexOf("Congratulations") != -1){
				System.out.println("TOR 브라우저를 사용합니다");
			}

		}catch(Exception e){

		}




		killFirefox();

	}

		private void killFirefox() {
		    Runtime rt = Runtime.getRuntime();

		    try {
		        rt.exec("taskkill /F /IM firefox.exe");
		        while (processIsRunning("firefox.exe")) {
		            Thread.sleep(100);
		        }
		    } catch (Exception e) {
		        e.printStackTrace();
		    }
		}

		private boolean processIsRunning(String process) {
		    boolean processIsRunning = false;
		    String line;
		    try {
		        Process proc = Runtime.getRuntime().exec("wmic.exe");
		        BufferedReader input = new BufferedReader(new InputStreamReader(proc.getInputStream()));
		        OutputStreamWriter oStream = new OutputStreamWriter(proc.getOutputStream());
		        oStream.write("process where name='" + process + "'");
		        oStream.flush();
		        oStream.close();
		        while ((line = input.readLine()) != null) {
		            if (line.toLowerCase().contains("caption")) {
		                processIsRunning = true;
		                break;
		            }
		        }
		        input.close();
		    } catch (IOException e) {
		        e.printStackTrace();
		    }
		    return processIsRunning;
		}

}
```

run만 눌러도 Tor브라우저가 뜸.

처음에는 프록시를 설정해주니까 오류가 떠서 주석처리했고,

실행할때 또 java heap space 에러가 떠서 스택오버플로우좀 찾아보니까 실행하는 메모리가 부족해서 그렇다길래

run-run configurations - Arguments - VM arguments에서 -Xms512m -Xmx1024m -XX:MaxPermSize=512m 를 입력했더니 잘 돌아간다.

TOR는 ff로 만들어진 브라우저이다. 그래서 어쨌거나 셀레니움으로 webdriver를 띄우려면 firefox로 띄워야한다.

이렇게 설정하면 동시에 tor브라우저, ff브라우저 동시에 2개가 뜨게되는데, 그냥 tor는 뜰때마다 토르인지아닌지 확인하라는것 같고, (아직 tor headless를 못찾음) ff는 headless로 해놓았다.

tor와 ff를 같이 사용하도록 설정해놓은거라 ff의 첫 url을 TOR를 사용했는지 아닌지를 확인하는 url로 설정해두었다.

이렇게 실행을 하게되면 Congratulations! 하면서 Tor브라우저를 사용하고 있다고 뜬다. 비록 ff로 뜨더라도 말이다.

나중에 좀더 tor브라우저와 셀레니움을 같이 사용하는 자세한 글을 쓰겠지만, 우선은 오늘은 코드만 작성하는걸로..

[토르 브라우저를 사용하는지 확인](https://check.torproject.org/) 하는 브라우저에 들어가보면 tor브라우저를 사용하는지 아닌지 확인할 수 있다.

Congratulations이 보이면 tor브라우저로 접속한거임. 그리고 그 뒤에 우회된 본인 아이피도 같이 나온다.

요즘은 이 체크페이지도 오류가 잦길래 내 코드에서는 몇번 확인하다가 빼버렸다.

우회하는건 확실하니말이다.
