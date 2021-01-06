import java.io.File;
import java.io.IOException;
import java.util.List;
import java.util.concurrent.TimeUnit;

import org.apache.commons.io.FileUtils;
import org.junit.Assert;
import org.openqa.selenium.By;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;
import org.testng.asserts.SoftAssert;

public class Accounts {
	WebDriver driver;
	@BeforeClass
	public void BeforeClassMethod()
	{
		System.setProperty("webdriver.chrome.driver", "C:\\IVS Files\\Selenium\\Drivers\\chromedriver v2.43\\chromedriver.exe");
		driver=new ChromeDriver();
		driver.get("http://10.82.180.36/Common/Login.aspx");
		driver.manage().window().maximize();
		driver.manage().timeouts().implicitlyWait(10000, TimeUnit.MICROSECONDS);
	}
	 @Test(priority=0)
	  public void guiCheckForCustomerMaster() throws IOException {
		 driver.findElement(By.id("body_txtUserID")).sendKeys("donhere");
		 driver.findElement(By.id("body_txtPassword")).sendKeys("don@123");
		 driver.findElement(By.id("body_btnLogin")).click();
		File scrFile = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);
		//Using the FileUtils class copy the generated screenshot file to any location
		FileUtils.copyFile(scrFile, new File("C:\\Users\\Desktop\\Screenshots\\CustomerMasterPage.png"));
	    
		  
	  }
	 @Test(priority=1)
	  public void guiCheckViewTransactions() throws IOException, InterruptedException {
		
		 driver.findElement(By.xpath("/html/body/form/div[3]/div[4]/div[1]/div/div/ul/li[4]/a")).click();
		File scrFile = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);
		//Using the FileUtils class copy the generated screenshot file to any location
		FileUtils.copyFile(scrFile, new File("C:\\Users\\Desktop\\Screenshots\\ViewTransactionsPage.png"));
		WebElement Accoutn_TypeDDL = driver.findElement(By.id("body_cph_MyAccount_ddlAccountNo"));
		
		Select ddl = new Select(Accoutn_TypeDDL);
		
		
		List<WebElement> allOptions = ddl.getOptions();
		int l=allOptions.size(); //here we are checking “Account No” drop down list is populated with all the active accounts of the  logged-in user
		                                                                                                    
		System.out.println(l);  
	  }
	 @Test(priority=2)
	  public void RadioButtonCheckofStatementType() throws IOException, InterruptedException {
		 WebElement we1=driver.findElement(By.id("body_cph_MyAccount_rblTransactions_0"));
		 we1.click();
		 System.out.println(we1.getText());
		 if(we1.getText().equals("Mini"))
		 {
			 File scrFile = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);
				//Using the FileUtils class copy the generated screenshot file to any location
				FileUtils.copyFile(scrFile, new File("C:\\Users\\\\Desktop\\Screenshots\\NoChanges.png"));
		 }
		 Thread.sleep(10000);
		 WebElement we2=driver.findElement(By.id("body_cph_MyAccount_rblTransactions_1"));
		 System.out.println(we2.getText());
		 we2.click();
		 Thread.sleep(3000);
			 WebElement we3=driver.findElement(By.id("body_cph_MyAccount_lblFromDate"));
			 WebElement we4=driver.findElement(By.id("body_cph_MyAccount_lblToDate"));
			 Thread.sleep(3000);
			 String str1=we3.getText();
			 String str2=we4.getText();
			 System.out.println(str1+"   "+str2);
			 if(str1.equals("From Date") && str2.equals("To Date"))
			 {
				 System.out.println("abc");
				 WebElement Accoutn_TypeDDL = driver.findElement(By.id("body_cph_MyAccount_ddlTransactionType"));
					
					Select ddl = new Select(Accoutn_TypeDDL);
					List<WebElement> allOptions = ddl.getOptions();
					String str3=allOptions.get(0).getText();
					Assert.assertEquals("--All--", str3);
					 File scrFile = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);
						//Using the FileUtils class copy the generated screenshot file to any location
						FileUtils.copyFile(scrFile, new File("C:\\Users\\Desktop\\Screenshots\\ViewTransactionswithDetailedradio.png"));
			 }
			
		 }
	 @Test(priority=3)
	  public void MandatoryCheck() throws IOException
	  {
		  driver.findElement(By.id("body_cph_MyAccount_btnViewTrancastions")).click();
		 String str1=driver.findElement(By.xpath("/html/body/form/div[3]/div[4]/div[2]/div[3]/table[1]/tbody/tr[1]/td[2]/table/tbody/tr/td[3]")).getText();
		 //SoftAssert sf=new SoftAssert();
		 Assert.assertEquals(str1,"Account No. is mandatory");
		 
	  }
	 @Test(priority=4)
	  public void MandatoryAcoountId() throws IOException, InterruptedException
	  {
		 WebElement we1=driver.findElement(By.id("body_cph_MyAccount_rblTransactions_0"));
		 we1.click();
		 Thread.sleep(3000);
		  driver.findElement(By.id("body_cph_MyAccount_btnViewTrancastions")).click();
			 String str1=driver.findElement(By.xpath("/html/body/form/div[3]/div[4]/div[2]/div[3]/table[1]/tbody/tr[1]/td[2]/table/tbody/tr/td[3]")).getText();
			// SoftAssert sf=new SoftAssert();
			 Assert.assertEquals(str1,"Account No. is mandatory");
			 Thread.sleep(3000);
		 
	  }
	 @Test(priority=5)
	  public void MandatoryStatementType() throws IOException, InterruptedException
	  {
		 driver.findElement(By.id("body_cph_MyAccount_btnReset")).click();
		 Thread.sleep(3000);
		 driver.findElement(By.id("body_cph_MyAccount_ddlAccountNo")).sendKeys("100000000000");
		 Thread.sleep(3000);
		  driver.findElement(By.id("body_cph_MyAccount_btnViewTrancastions")).click();
			 String str1=driver.findElement(By.xpath("/html/body/form/div[3]/div[4]/div[2]/div[3]/table[1]/tbody/tr[3]/td[3]/table/tbody/tr/td[3]")).getText();
//			 SoftAssert sf=new SoftAssert();
			 System.out.println(str1);
			Assert.assertEquals(str1,"Transaction type is mandatory");
		 
	  }
	 
	 @Test(priority=6)
	  public void ViewTransactionClicked() throws IOException, InterruptedException
	  {
		 driver.findElement(By.id("body_cph_MyAccount_btnReset")).click();
		 Thread.sleep(3000);
		 driver.findElement(By.id("body_cph_MyAccount_ddlAccountNo")).sendKeys("100000000000");
		 driver.findElement(By.id("body_cph_MyAccount_rblTransactions_0")).click();
		 Thread.sleep(3000);
		 driver.findElement(By.id("body_cph_MyAccount_btnViewTrancastions")).click();
		 Thread.sleep(3000);
		 File scrFile = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);
			//Using the FileUtils class copy the generated screenshot file to any location
			FileUtils.copyFile(scrFile, new File("C:\\Users\\Desktop\\Screenshots\\ViewTransactionsClicked.png"));
	  }
	 
 
  @AfterClass
  public void afterClassMethod()
  {
	  driver.close();
	  driver.quit();
  }
}







