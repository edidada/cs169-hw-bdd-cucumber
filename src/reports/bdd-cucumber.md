# CS169 hw-bdd-cucumber（CHIP 7.7 BDD + Cucumber）

## 已通过的公开验证（2026-11-08）

验收分支 `edidada/cs169-hw`，最终 HEAD `a2a25f4d45933be5962a164fadc8c1f159fe9958`，[Actions run](https://github.com/edidada/cs169-hw-bdd-cucumber/actions)（branch push 成功）：官方 canonical controller 的 4 个 scenario 全部通过：

- filter_movie_list.feature：
  - restrict to "PG" or "R" ratings——uncheck G/PG-13、Refresh、5 部 PG/R 可见、5 部其他不可见
  - all ratings selected——默认全 4 rating 打勾直接 Refresh，10 部电影均可见
- sort_movie_list.feature：
  - 按字母排序（Aladdin 在 The Terminator/When Harry Met Sally 前，Chicken Run 在 Raiders 前）
  - 按发行日升序（2001: A Space Odyssey 在 Aladdin 前、The Terminator 在 Chicken Run 前、Aladdin 在 The Help 前）

## 实现

- `features/step_definitions/movie_steps.rb`：官网骨架中五个 pending 步骤全部落实——
  - `the following movies exist` 用 `Movie.create!` 导入表格行；
  - `(n) seed movies should exist` 断言计数；
  - `I should see "(.*)" before "(.*)" in the movie list` 用 page.body 序号比较；
  - `I (un)check the following ratings: (.*)` 使用 Capybara `checkbox.set(true/false)` 直接勾选/取消（比 web_steps 的标签定位可靠）；
  - `I should (not )?see the following movies: (.*)`/`I should see all the movies` 逐条内容断言。
- `features/*.feature` 中的 scenario 主体按官方提示补全（官方 controller/view 未改动；官方 rating 默认全勾语义被 'all ratings selected' 场景利用）。

## CI 修复史（保留）

- Capybara `uncheck(checkbox)` 元素型调用的弃用警告→元素 `set(true/false)` 替代；
- 发行日排序 scenario 一度按错误年份假设书写,与官方数据不符 → 按真实日期序列修改。

## 边界与局限

- 这是 HW 官方 canonical 集成的验收；不宣称 SAAS edx autograder 覆盖，也不跨出 scenario 断言以外的额外行为保证。
- 本地仅静态核（ruby -c），所有 Cucumber 执行都在 GitHub Actions。
