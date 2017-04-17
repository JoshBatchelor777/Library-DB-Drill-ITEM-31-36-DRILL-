# Library-DB-Drill-ITEM-31-36-DRILL-
A Library Database with each drill question attached.



--
--	ANSWERS TO LAB DRIL FROM ITEM 30/36: DATABASES & SQL
--


--QUESTION 1:

--Select statement for quantifying the amount of copies of the
--book titled, "The Lost Tribe" is owned by the library branch
--called "Sharpstown".

CREATE PROC GetTitle
AS

SELECT count(*)
FROM LibraryBranch AS lb
JOIN Book AS b
ON lb.b_BranchID = b.b_BookID
WHERE b.b_BookID = 1
AND lb.b_BranchName = 'Sharpstown'

EXEC GetTitle

--QUESTION 2:

--A query to see how many copies of "The Lost Tribe" are owned by
--each branch.

SELECT count(*)
FROM LibraryBranch AS lb
JOIN Book AS b
ON lb.b_BranchID = b.b_BookID
WHERE b.b_Title = 'The Lost Tribe'



--QUESTION 3:

--A query to see the names of everyone who does not
--have a book checked out to them.

SELECT bo.b_CardNO, bo.b_Name, bl.b_DateOut, bl.b_DueDate
FROM BookLoans AS bl 
JOIN Borrower AS bo ON bl.b_CardNo = bo.b_CardNo
WHERE b_DateOut IS NULL;


--QUESTION 4:

--A query to see what book is due today (I'm using the date 04-13

-17 for
--'Today'), and to see the name of whom the book is loaned out to.

SELECT lb.b_BranchName, bo.b_Title, bor.b_Name, bor.b_Address, 

bl.b_DateOut, bl.b_DueDate
FROM Book AS bo
JOIN LibraryBranch AS lb ON bo.b_BookID = lb.b_BranchID
JOIN BookLoans AS bl ON lb.b_BranchID = bl.b_BranchID
JOIN Borrower AS bor ON bor.b_CardNo = lb.b_BranchID
WHERE lb.b_BranchName = 'Sharpstown'
AND bl.b_DueDate = '04-13-17'
ORDER BY b_BranchName ASC;


--QUESTION 5:

--A query to see how many books are loaned out to each Library 

Branch,
--and the names of the branches.

SELECT 
	( --By doing it all this way, I am able to see exactly
	  --what branches have what values. I assigned a name
	  --to each numeric output.
	SELECT count(*)
	FROM LibraryBranch lb 
	JOIN BookLoans bl ON lb.b_BranchID = bl.b_BranchID
	WHERE lb.b_BranchName = 'Sharpstown'
	) AS Sharpstown,
	(
	SELECT count(*)
	FROM LibraryBranch lb 
	JOIN BookLoans bl ON lb.b_BranchID = bl.b_BranchID
	WHERE lb.b_BranchName = 'Beaverton Library'
	) AS Beaverton_Library,
	(
	SELECT count(*)
	FROM LibraryBranch lb 
	JOIN BookLoans bl ON lb.b_BranchID = bl.b_BranchID
	WHERE lb.b_BranchName = 'Powels Book Store'
	) AS Powels_Book_Store,
	(
	SELECT count(*)
	FROM LibraryBranch lb 
	JOIN BookLoans bl ON lb.b_BranchID = bl.b_BranchID
	WHERE lb.b_BranchName = 'Central'
	) AS Central,
	(
SELECT count(*)
FROM BookLoans
) AS Total_Loans


--QUESTION 6:

--A query to return all the information (name, address, number of
--books loaned) of the borrowers who have more than 5 books out.

SELECT bor.b_Name, bor.b_Address, count(*)
FROM BookLoans bl 
JOIN Borrower bor ON bl.b_BranchID = bor.b_CardNo
JOIN Book bo ON bo.b_BookID = bor.b_BookID
GROUP BY bor.b_Address, bor.b_Name
HAVING COUNT(*) > 5

--Question 7:


--A query to retrieve the number of copies of every book authored 

by Stephen
--King, owned by the Central library branch.


SELECT * 
FROM BookAuthors ba
JOIN Book bo ON ba.b_BookID = bo.b_BookID
WHERE ba.b_AuthorName = 'Stephen King'

SELECT 
	(
	SELECT count(*)
	FROM LibraryBranch lb 
	JOIN BookAuthors ba ON lb.b_BranchID = ba.b_BookID
	WHERE lb.b_BranchName = 'Central'
	AND ba.b_AuthorName = 'Stephen King'
) AS BookAmount



--STORED PROCEDURE

CREATE PROC GetTitle
AS

SELECT count(*)
FROM LibraryBranch AS lb
JOIN Book AS b
ON lb.b_BranchID = b.b_BookID
WHERE b.b_BookID = 1
AND lb.b_BranchName = 'Sharpstown'

EXEC GetTitle





