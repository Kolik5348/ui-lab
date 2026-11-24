# UI lab results

Tool parts that I chose: Part 2 (Selenium WebDriver in Java) and Part 4 (Cypress)

## Part 0
- Step 0.1, Chrome version: 157.0.8103.79
- Step 0.3, ChromeDriver version: ChromeDriver 157.0.8103.79
- Step 0.4, locator of Pizza, Please!: name=pizza-please
- Step 0.4, locator of Excluded tools: select
- Step 0.4, locator of Must be vegetarian: id=default-checkbox
- Step 0.4, locator of Custom Pizza Name: input#pizza-name
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261124-152700, Pizza cutter, 600 calories per slice

| Step | Actual result                                                                                       | Pass or fail |
|------|-----------------------------------------------------------------------------------------------------|--------------|
| 1    | Heading reads "Looking to break out of your pizza routine?"                                         | Pass |
| 2    | Max Calories per Slice, Excluded tools, Must be vegetarian and Custom Pizza Name appeared           | Pass |
| 3    | Fields show 600, Knife, vegetarian ticked and the name Lab Pizza 20261124-152700                    | Pass |
| 4    | Answer shows Lab Pizza 20261124-152700, tool Pizza cutter, 600 calories per slice                   | Pass |
| 5    | Page says "Please log in first."                                                                    | Pass |
| 6    | After signing in with admin/admin, the page shows "Latest pizza recommendations"                    | Pass |
| 7    | Line shows Lab Pizza 20261124-152700 (tool=Pizza cutter, ingredients_number=8), same tool as step 4 | Pass |

## Part 2: Selenium WebDriver in Java
- Step 2.4, the failure line for the wrong title: [ERROR]   FirstTest.webFormOpens:20 expected: <Web Form> but was: <Web form>
- Step 2.15, QP01Test run 1, result and time: Pass, 12.931 s
- Step 2.15, QP01Test run 2, result and time: Pass, 11.510 s
- Step 2.20, full run 1, time and number of tests: 24.842 s, 5 tests
- Step 2.20, full run 2, time and number of tests: 24.432 s, 5 tests

## Part 4: Cypress
- node --version, npm --version and npx cypress version: v24.14.1, 11.11.0, 16.2.1
- Step 4.3, failure message for the wrong title: + expected - actual, -'Web form', +'Web Form'
- Step 4.6, tool and calories in the answer of POST /api/pizza: tool: Knife, calories : 600
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261124-232715 (tool=Pizza cutter, ingredients_number=5)
- Step 4.8, run 1, result, duration and pizza name: Pass, 00:13, Lab Pizza 20261124-232353
- Step 4.8, run 2, result, duration and pizza name: Pass, 00:12, Lab Pizza 20261124-232715

## Part B
1. The three number boxes of the advanced section share one id, floating_filled, and the Console count in Step 0.4 returned 3, so an id cannot pick the calories box. The label text "Max Calories per Slice" is unique, so the tests find that label and take the input next to it (preceding-sibling in Selenium, .siblings('input') in Cypress). This works, but it depends on the wording and the structure of the page. If the developers gave each box its own id, such as max-calories, or a data-testid attribute, a test could use a short and direct locator that survives changes of layout and text. That would also make the locator easier to write and to review.

2. The check compares the tool in the admin list with the tool in the recommendation, and no expected value is written in advance, because the value comes from an earlier output of the system itself. It is therefore a consistency oracle (a kind of pseudo-oracle), which checks two outputs of the system against each other and not against a specified correct value. It would catch a fault where the server saves the pizza with a different tool than it recommended, for example (tool=Knife) in the list but Pizza cutter in the answer. It would also expose an admin page that shows an old record or a pizza from another server. It cannot tell whether the tool is right for the restrictions, which is why step 4 checks separately that the tool is not Knife.

3. The answer of POST /api/pizza has a field vegetarian and a list of ingredients, so a Cypress test could check expect(response.body.vegetarian).to.equal(true) through cy.intercept. That trusts only the flag of the server, so a stronger check also needs an independent source, such as a list of ingredients known to be non-vegetarian, and the test would check that no ingredient of the pizza is on that list. For this the test needs the ingredient names, either from the response body or from the page, and the list from the requirements or from the product owner. A Selenium test would need the page to show the ingredients or the flag, because a test can only check what the page makes readable.

4. In Selenium I wrote the waits myself with WebDriverWait and ExpectedConditions, for example textToBePresentInElementLocated for the answer, and WaitTest failed with NoSuchElementException for #box0 until I added a wait. In Cypress I wrote no wait at all, because every cy.get and should tries again for up to 10 seconds, a limit that is set once in cypress.config.js. QP-01 took fewer lines in Cypress, one spec of about 80 lines, than in Selenium, where QP01Test has about 100 lines and the page objects and BaseTest come on top, although the page objects pay off when tests share pages. The Selenium report was plain Maven text such as Tests run: 5, and the screenshots needed code that I wrote, while Cypress gave a table per spec, a Command Log and one mp4 video per spec, which I found easier to read. The run times were about the same, 12.9 s and 11.5 s for QP01Test against 13 s and 12 s for pizza-scenario.cy.js.

5. I would use Cypress for Project 2, for two reasons from this lab. First, it waits for elements by itself: my Cypress spec has no wait lines and no choice of wait condition for each step, while in Selenium every new step needed a WebDriverWait, and the failure of WaitTest showed what happens without one. Second, it gives better evidence and a deeper check with less work: video and screenshots come from one setting and one line, and cy.intercept let me prove that the request carried maxCaloriesPerSlice 600 and that the page showed the same tool and calories as the server sent. The limit of one site per test does not matter for Project 2, because its tests visit one site.

6. In QP01Test the locator of the calories box is written inside the test method, as the XPath of step 2, so I would edit that string there, and in every other test that uses the box. In QP01PageObjectTest nothing changes, because only the constant MAX_CALORIES in HomePage holds the locator, and showAdvanced, setMaxCalories and maxCalories all use that constant. The page objects therefore make the repair one line in one file, however many tests use the box. The test file also keeps reading as the steps of QP-01 and does not fill up with locators.

7. Cypress clears the cookies before each test, and QuickPizza runs on several servers, where a cookie keeps one browser on one server and the admin list shows only the pizzas of that server. If steps 6 and 7 were a second test, the browser would arrive with no cookie and could land on another server, whose admin list does not hold my pizza, so step 7 could fail even though the application is right. The second test would also lose the pizzaName constant, which belongs to the first test, and the alias tool, because Cypress clears aliases between tests, so it could not know which pizza and which tool to look for. Keeping all seven steps in one test keeps the same browser session and the same data from step 1 to step 7.