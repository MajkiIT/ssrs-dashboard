package - executions group by time range, reportid

folder - folder

execution - execution

? - subscription

executable - report

project - report folder (first parent)

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
