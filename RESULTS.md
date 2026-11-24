# UI lab results

Tool parts that I chose: Part 2 (Selenium WebDriver in Java) and Part ?

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
- Step 4.6, tool and calories in the answer of POST /api/pizza:
- Step 4.7, the line of the admin list that shows your pizza:
- Step 4.8, run 1, result, duration and pizza name:
- Step 4.8, run 2, result, duration and pizza name:

## Part B
1.
2.
3.
4.
5.
6.
7.