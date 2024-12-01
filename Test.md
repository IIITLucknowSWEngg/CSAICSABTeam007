# Features for YouTube Platform

---

## Feature: User - Sign In to Google Account

### Scenario: User signs in to their Google account

#### Given:
- The user is on the Google sign-in page.

#### When:
- The user enters valid Google account credentials (email and password).  
- The user clicks the "Sign In" button.

#### Then:
- The user should be signed in successfully.  
- The user should be redirected to the YouTube homepage.

```javascript
const chai = require('chai');
const expect = chai.expect;
const googleSignInPage = require('../pages/googleSignInPage');

describe('User - Sign In to Google Account', function() {
  it('should allow the user to sign in successfully', function() {
    googleSignInPage.open();
    googleSignInPage.enterCredentials('user@example.com', 'securepassword123');
    googleSignInPage.submitSignIn();
    expect(googleSignInPage.getWelcomeMessage()).to.include('Welcome, User');
    expect(browser.getUrl()).to.include('/youtube');
  });
});
```

---

## Feature: Content Creator - Video Upload

### Scenario: Content Creator uploads a video successfully

#### Given:
- The content creator is logged in.  
- The content creator is in **YouTube Studio** on the video upload page.

#### When:
- The content creator selects a valid video file.  
- The content creator fills out the video title, description, and tags.  
- The content creator clicks the upload button.

#### Then:
- The video should be successfully uploaded.  
- The video should appear on the content creator's channel.
  
```javascript
const chai = require('chai');
const expect = chai.expect;
const studioPage = require('../pages/studioPage');

describe('Content Creator - Video Upload', function() {
  it('should allow content creator to upload a video successfully', function() {
    studioPage.open();
    studioPage.uploadVideo('video.mp4', 'Tutorial: Learn JavaScript', 'A complete guide to JavaScript.', ['JavaScript', 'Tutorial']);
    expect(studioPage.getSuccessMessage()).to.equal('Video uploaded successfully');
    expect(studioPage.getUploadedVideoTitle()).to.equal('Tutorial: Learn JavaScript');
  });
});
```
---

## Feature: User - Watch and Interact with Videos

### Scenario: User watches a video and likes it

#### Given:
- The user is logged in.  
- The user is on the video page.

#### When:
- The user watches the video.  
- The user clicks the like button.

#### Then:
- The like count of the video should increase by one.  
- The video should be added to the user's liked videos.

### Chai.js Code:

```javascript
const chai = require('chai');
const expect = chai.expect;
const videoPage = require('../pages/videoPage');
const userPage = require('../pages/userPage');

describe('User Watch and Like Video', function() {
  it('should increase the like count and add video to liked videos', function() {
    videoPage.open();
    videoPage.clickPlayButton();  
    videoPage.clickLikeButton();  
    const likeCount = videoPage.getLikeCount();
    expect(likeCount).to.be.above(0);  

    userPage.openLikedVideos();
    const likedVideos = userPage.getLikedVideos();
    expect(likedVideos).to.include(videoPage.getVideoTitle());  
  });
});
```
---
## Feature: User - Subscribe to a Channel

### Scenario: User subscribes to a channel

#### Given:
- The user is logged in.  
- The user is on the channel page of a content creator.

#### When:
- The user clicks the "Subscribe" button.

#### Then:
- The user should be subscribed to the channel.  
- The "Subscribe" button should change to "Subscribed."

### Chai.js Code:

```javascript
const chai = require('chai');
const expect = chai.expect;
const channelPage = require('../pages/channelPage');
const userPage = require('../pages/userPage');

describe('User Subscribe to Channel', function() {
  it('should subscribe user to the channel and change button text', function() {
    channelPage.open();  
    const initialButtonText = channelPage.getSubscribeButtonText();
    expect(initialButtonText).to.equal('Subscribe');  

    channelPage.clickSubscribeButton();  

    const updatedButtonText = channelPage.getSubscribeButtonText();
    expect(updatedButtonText).to.equal('Subscribed');  

    const isSubscribed = userPage.isChannelSubscribed(channelPage.getChannelName());
    expect(isSubscribed).to.be.true;  
  });
});
```
---
## Feature: User - Share Video

### Scenario: User shares a video

#### Given:
- The user is logged in.  
- The user is on a video page.

#### When:
- The user clicks the "Share" button.  
- The user selects a sharing option (e.g., via email, social media).

#### Then:
- The video should be shared via the selected method.

### Chai.js Code:

```javascript
const chai = require('chai');
const expect = chai.expect;
const videoPage = require('../pages/videoPage');
const sharePage = require('../pages/sharePage');

describe('User Share Video', function() {
  it('should share the video via the selected method', function() {
    videoPage.open();  
    videoPage.clickShareButton();  

    sharePage.selectShareOption('Email');  
    sharePage.submitShare();  

    const shareConfirmation = sharePage.getShareConfirmation();
    expect(shareConfirmation).to.equal('Video shared successfully via Email');  

    expect(sharedLinks).to.include(videoPage.getVideoUrl());  
  });
});
```
---
## Feature: User - Report Video

### Scenario: User reports a video

#### Given:
- The user is logged in.  
- The user is on a video page.

#### When:
- The user clicks on the "More" button and selects "Report."  
- The user selects a reason for reporting the video.

#### Then:
- The video should be reported for review by YouTube's moderation team.
```javascript
const chai = require('chai');
const expect = chai.expect;
const videoPage = require('../pages/videoPage');
const reportPage = require('../pages/reportPage');

describe('User - Report Video', function() {
  it('should report the video successfully', function() {
    videoPage.open();  
    videoPage.clickMoreButton();  
    videoPage.selectReportOption();  

    reportPage.selectReportReason('Inappropriate content');  
    reportPage.submitReport(); 

    const confirmationMessage = reportPage.getConfirmationMessage();
    expect(confirmationMessage).to.equal('Video reported successfully');  
  });
});

```
---

## Feature: Content Creator - View Analytics

### Scenario: Content Creator views video analytics

#### Given:
- The content creator is logged in.  
- The content creator is on the **YouTube Studio** page.

#### When:
- The content creator clicks on the "Analytics" tab for a specific video.

#### Then:
- The content creator should see the video’s analytics, including views, watch time, and audience demographics.

```javascript
const chai = require('chai');
const expect = chai.expect;
const analyticsPage = require('../pages/analyticsPage');

describe('Content Creator - View Analytics', function() {
  it('should display video analytics to the content creator', function() {
    analyticsPage.open('video_id_123');
    const metrics = analyticsPage.getVideoMetrics();
    expect(metrics).to.have.property('views').that.is.a('number');
    expect(metrics).to.have.property('likes').that.is.a('number');
    expect(metrics).to.have.property('comments').that.is.a('number');
    expect(metrics).to.have.property('watchTime').that.is.a('number');
  });
});
```
---

## Feature: Content Creator Earnings View

### Scenario: Verify Earnings Dashboard Details

#### Given:
- The content creator is logged into YouTube Studio
- The content creator is on the Earnings page

#### When:
- The content creator clicks on the "Earnings" tab

#### Then:
- Total earnings should be visible
- Total earnings should display in correct currency format
- Revenue sources section should be displayed
- At least one revenue source should exist
- Payment history table should be visible
- Payment history table should have at least one entry

```javascript
import { expect } from 'chai';

describe('Content Creator Earnings View', () => {
  let page;

  beforeEach(async () => {
    page = await browser.newPage();
    await page.goto('https://studio.youtube.com/earnings');
    await page.login('contentcreator@example.com', 'securePassword');
  });

  afterEach(async () => {
    await page.close();
  });

  it('should display earnings information when Earnings tab is clicked', async () => {
    await page.click('[data-testid="earnings-tab"]');

    const totalEarningsElement = await page.locator('[data-testid="total-earnings"]');
    expect(await totalEarningsElement.isVisible()).to.be.true;
    expect(await totalEarningsElement.textContent()).to.match(/^\$\d+(\.\d{1,2})?$/);

    const revenueSources = await page.locator('[data-testid="revenue-sources"]');
    expect(await revenueSources.isVisible()).to.be.true;
    const sourceItems = await revenueSources.$$('[data-testid="revenue-source-item"]');
    expect(sourceItems.length).to.be.greaterThan(0);

    const paymentHistoryTable = await page.locator('[data-testid="payment-history-table"]');
    expect(await paymentHistoryTable.isVisible()).to.be.true;
    const paymentRows = await paymentHistoryTable.$$('tr');
    expect(paymentRows.length).to.be.greaterThan(0);
  });
});
```
