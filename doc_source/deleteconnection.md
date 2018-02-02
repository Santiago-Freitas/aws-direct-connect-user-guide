# Deleting a Connection<a name="deleteconnection"></a>

You can delete a connection as long as there are no virtual interfaces attached to it\. Deleting your connection stops all port hour charges for this connection\. AWS Direct Connect data transfer charges are associated with virtual interfaces\. Any cross connect or network circuit charges are independent of AWS Direct Connect and must be cancelled separately\. For more information about how to delete a virtual interface, see [Deleting a Virtual Interface](deletevif.md)\.

If the connection is part of a link aggregation group \(LAG\), you cannot delete the connection if doing so will cause the LAG to fall below its setting for minimum number of operational connections\. 

**To delete a connection**

1. Open the AWS Direct Connect console at [https://console\.aws\.amazon\.com/directconnect/](https://console.aws.amazon.com/directconnect/)\.

1. If necessary, change the region in the navigation bar\. For more information, see [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html)\.

1. In the navigation pane, choose **Connections**\.

1. In the **Connections** pane, select the connection to delete, and then choose **Actions**, **Delete Connection**\.

1. In the **Delete Connection** dialog box, choose **Delete**\.

**To delete a connection using the command line or API**

+ [delete\-connection](http://docs.aws.amazon.com/cli/latest/reference/directconnect/delete-connection.html) \(AWS CLI\)

+ [DeleteConnection](http://docs.aws.amazon.com/directconnect/latest/APIReference/API_DeleteConnection.html) \(AWS Direct Connect API\)