# 使用 SQL Server Agent 排程執行 Stored Procedure

1. 先確認你的 sql 登入的權限是否可以開啟「SQL Server Agent」

    ![clipboard](https://i.imgur.com/NrAsMz2.png)

2. 然後在「SQL Server Agent」上按滑鼠右鍵，選擇「新增」、「作業」。

    ![clipboard](https://i.imgur.com/fGb783u.png)

    接下來會顯示「新增作業」視窗，請在「名稱」的部份輸入這個作業的名稱

    ![clipboard](https://i.imgur.com/Sh11xaO.png)

3. 接下來點選「步驟」，按「新增」按鈕。

    ![clipboard](https://i.imgur.com/dKfJhIj.png)

    然後會顯示「新增作業步驟」視窗，請依照下列設定，記得資料庫要選對哦

    * 類型要選 Transact-SQL指令碼(T-SQL)
    * 然後在命令的部份輸入執行 stored procedure 的語法
    * 最後後按下「確定」按鈕

        ![clipboard](https://i.imgur.com/9Zn70K1.png)

4. 接下來我們要設定排程，讓這個動作可以由電腦重覆執行。請點選「排程」，按下「新增」按鈕

    ![clipboard](https://i.imgur.com/AI6Y6XZ.png)

    然後會顯示「新增作業排程」視窗，請輸入排程名稱、選擇「排程類型」、指定重複執行的頻率以及每日執行的時間，設定完之後，按下「確定」按鈕。

    ![clipboard](https://i.imgur.com/vPhQxNh.png)

5. 最後再按下「確定」就完成了

    ![clipboard](https://i.imgur.com/GgtvseU.png)

6. 完成後，依序展開「SQL Server Agent」、「作業」節點，應該能看到我們剛剛加入的作業。

    ![clipboard](https://i.imgur.com/QGkjNHM.png)


