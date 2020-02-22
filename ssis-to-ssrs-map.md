package - executions group by time range, reportid

folder - folder (Type 1, "/")

? - subfolder (Type 1, "/.../...")

execution - execution

? - subscription

executable - report

project - path = report folder (first parent) (Type 2 and remove report name from path)

package history - ExecutonLogStorage

running:

SELECT 
		c.Name
	   ,sub.Description
      ,[TotalNotifications]
      ,[TotalSuccesses]
      ,[TotalFailures]
	  ,sub.LastRunTime
	  ,sub.DeliveryExtension
	  ,ntf.DeliveryExtension
	  ,ntf.BatchID
	  ,ntf.ProcessHeartbeat
	  ,ntf.NotificationEntered
	  ,ntf.ProcessStart
	  ,ntf.ProcessAfter
  FROM [ReportServer].[dbo].[ActiveSubscriptions] as act
  inner join [ReportServer].[dbo].[Subscriptions] sub
  on act.SubscriptionID=sub.SubscriptionID
  inner join  [ReportServer].[dbo].[Notifications] ntf
  on ntf.SubscriptionID=sub.SubscriptionID 
  inner join [ReportServer].[dbo].[Catalog] c
  on c.ItemID = ntf.ReportID
  
  packages:
  
    Select 
   Reportid,
   DATEADD(MINUTE, DATEDIFF(MINUTE, '2020', timestart) / 15 * 15, '2020') AS [timestartrange], 
   status,
   COUNT(*)
   from  [ReportServer].[dbo].[ExecutionLogStorage]
  where TimeStart>=GETDATE()-2
  group by  Reportid,   DATEADD(MINUTE, DATEDIFF(MINUTE, '2020', timestart) / 15 * 15, '2020'), status
