star
====
@@ -0,0 +1,72 @@
1	+package com.fuzionsoftware.appstats;
2	+

	2	+
3	+import java.util.ArrayList;
4	+import java.util.HashMap;
5	+import java.util.List;
6	+
7	+import android.app.ExpandableListActivity;
8	+import android.content.pm.PackageInfo;
9	+import android.content.pm.PackageManager;
10	+import android.os.Bundle;
11	+import android.view.MenuItem;
12	+import android.widget.ExpandableListAdapter;
13	+import android.widget.ExpandableListView;
14	+import android.widget.ExpandableListView.ExpandableListContextMenuInfo;
15	+import android.widget.TextView;
16	+import android.widget.Toast;
17	+
18	+public class ApplicationStatsActivity extends ExpandableListActivity {
19	+
20	+    ExpandableListAdapter mAdapter;
21	+
22	+    @Override
23	+    public void onCreate(Bundle savedInstanceState) {
24	+        super.onCreate(savedInstanceState);
25	+        HashMap<String,ArrayList<PackageInfo>> hMap = getPermissionAppMap();
26	+        mAdapter = new PermissionAppExpandableAdapter(hMap,this);
27	+        setListAdapter(mAdapter);
28	+    }
29	+
30	+    public HashMap<String,ArrayList<PackageInfo>> getPermissionAppMap()
31	+    {
32	+    	//Permission as the key, array of packages that use that permission
33	+    	HashMap<String,ArrayList<PackageInfo>> permissionAppmap = new HashMap<String,ArrayList<PackageInfo>>(128);    	
34	+    	PackageManager pm  = getPackageManager();
35	+    	List<PackageInfo> packages = pm.getInstalledPackages(PackageManager.GET_PERMISSIONS);
36	+    	
37	+    	for(PackageInfo pi : packages)
38	+    	{
39	+    		if (pi.requestedPermissions == null)
40	+    			continue;
41	+    		for(String permission : pi.requestedPermissions)
42	+    		{
43	+	    		if (!permissionAppmap.containsKey(permission))
44	+	    		{
45	+	    			ArrayList<PackageInfo> arPi = new ArrayList<PackageInfo>(20);
46	+	    			permissionAppmap.put(permission, arPi);
47	+	    		}
48	+    			permissionAppmap.get(permission).add(pi);
49	+    		}
50	+    	}    
51	+    	return permissionAppmap;
52	+    }
53	+    @Override
54	+    public boolean onContextItemSelected(MenuItem item) {
55	+        ExpandableListContextMenuInfo info = (ExpandableListContextMenuInfo) item.getMenuInfo();
56	+        String title = ((TextView) info.targetView).getText().toString();
57	+        int type = ExpandableListView.getPackedPositionType(info.packedPosition);
58	+        if (type == ExpandableListView.PACKED_POSITION_TYPE_CHILD) {
59	+            int groupPos = ExpandableListView.getPackedPositionGroup(info.packedPosition); 
60	+            int childPos = ExpandableListView.getPackedPositionChild(info.packedPosition); 
61	+            Toast.makeText(this, title + ": Child " + childPos + " clicked in group " + groupPos,
62	+                    Toast.LENGTH_SHORT).show();
63	+            return true;
64	+        } else if (type == ExpandableListView.PACKED_POSITION_TYPE_GROUP) {
65	+            int groupPos = ExpandableListView.getPackedPositionGroup(info.packedPosition); 
66	+            Toast.makeText(this, title + ": Group " + groupPos + " clicked", Toast.LENGTH_SHORT).show();
67	+            return true;
68	+        }
69	+
70	+        return false;
	71	+    }
* }
