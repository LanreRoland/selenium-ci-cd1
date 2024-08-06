const { Builder, By, Key, until } = require('selenium-webdriver');

async function runTests() {
  let driver = await new Builder().forBrowser('chrome').build();
  try {
    // Successful Login
    await driver.get('https://opensource-demo.orangehrmlive.com/index.php/auth/login');
    await driver.findElement(By.name('username')).sendKeys('Admin');
    await driver.findElement(By.name('password')).sendKeys('admin123', Key.RETURN);
    await driver.wait(until.elementLocated(By.css('#branding > img')), 10000);
    let brandImage = await driver.findElement(By.css('#branding > img')).getAttribute('alt');
    console.log('Login successful. Brand image alt:', brandImage);

    // Invalid Login
    await driver.get('https://opensource-demo.orangehrmlive.com/index.php/auth/login');
    await driver.findElement(By.name('username')).sendKeys('Admin');
    await driver.findElement(By.name('password')).sendKeys('wrongpassword', Key.RETURN);
    await driver.wait(until.elementLocated(By.css('.oxd-alert-content > .oxd-text')), 10000);
    let errorMessage = await driver.findElement(By.css('.oxd-alert-content > .oxd-text')).getText();
    console.log('Invalid login error message:', errorMessage);

    // Display Error Message on Failed Login
    await driver.get('https://opensource-demo.orangehrmlive.com/index.php/auth/login');
    await driver.findElement(By.name('username')).sendKeys('Admin');
    await driver.findElement(By.name('password')).sendKeys('wrongpassword', Key.RETURN);
    await driver.wait(until.elementLocated(By.css('.oxd-alert-content > .oxd-text')), 10000);
    let failureMessage = await driver.findElement(By.css('.oxd-alert-content > .oxd-text')).getText();
    if (failureMessage.includes('Invalid credentials')) {
      console.log('Correct error message displayed: Invalid credentials');
    } else {
      console.log('Error message not as expected:', failureMessage);
    }
  } finally {
    await driver.quit();
  }
}

runTests();
