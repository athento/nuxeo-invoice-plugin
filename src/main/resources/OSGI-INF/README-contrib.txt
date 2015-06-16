1 First of all, we have to define the schemas:
- Invoice.xsd defines the type itself
- Invoice_Search_cv defines the fields used in our custom search and is a Content View (cv)

2 In types-contrib.xml we contribute our new schemas
	<extension target="org.nuxeo.ecm.core.schema.TypeService" point="schema">
		<schema name="Invoice_Search_cv" prefix="Invoice_Search_cv" src="data/schemas/Invoice_Search_cv.xsd"/>
		<schema name="Invoice" prefix="Invoice" src="data/schemas/Invoice.xsd"/>
	</extension>
	
	We also define the 2 Invoce doctypes
	<extension target="org.nuxeo.ecm.core.schema.TypeService" point="doctype">
		<doctype name="Invoice" extends="File">
			<schema name="Invoice"/>
		</doctype>
		<doctype name="Invoice_Search_cv" extends="Document">
			<facet name="ContentViewDisplay"/>
			<facet name="SavedSearch"/>
			<facet name="HiddenInNavigation"/>
			<schema name="Invoice_Search_cv"/>
		</doctype>
	</extension>
	
	And the Invoice type (with its layouts) and types implied:
	<extension target="org.nuxeo.ecm.platform.types.TypeService" point="types">
		<type id="Invoice">
			<label>Invoice</label>
			<icon>/img/file_100.png</icon>
			<bigIcon>/img/file_100.png</bigIcon>
			<description>Invoice</description>
			<default-view>view_documents</default-view>
			<layouts mode="create">
				<layout>layout@Invoice-create</layout>
			</layouts>
			<layouts mode="edit">
				<layout>layout@Invoice-edit</layout>
			</layouts>
			<layouts mode="view">
				<layout>layout@Invoice-view</layout>
			</layouts>
		</type>
		<type id="Invoice_Search_cv">
			<label>DefaultSearch</label>
			<icon>/icons/search.png</icon>
			<bigIcon>/icons/search_100.png</bigIcon>
			<description>DefaultSearch.description</description>
			<default-view>home_view_documents</default-view>
			<layouts mode="any">
				<layout>heading</layout>
				<layout>Invoice_Search_cv@search</layout>
			</layouts>
		</type>
		<type id="Folder">
			<subtypes>
				<type>Invoice</type>
			</subtypes>
		</type>
		<type id="Workspace">
			<subtypes>
				<type>Invoice</type>
			</subtypes>
		</type>

	</extension>

	3 Then, we need to define the layouts implied. Look at weblayout-layouts-contrib.xml
	NOTE: maybe your layout and widgets for create and edit mode are the same. 
	You could reference the same instead of writting 2 layouts
	
	4 For widgets defined in layouts, you may need some vocabularies. 
	See them in SQLDirectories-vocabularies-contrib.xml. 
	Names defined here are used in widget properties
			<widget name="Invoice_type" type="selectOneDirectory">
				<labels>
					<label mode="any">label.factura.type</label>
				</labels>
				<translated>true</translated>
				<fields>
					<field>Invoice_Search_cv:Invoice_type</field>
				</fields>
				<properties widgetMode="edit">
					<property name="localize">true</property>
					<property name="directoryName">invoiceTypes</property>
					<property name="ordering">ordering</property>
				</properties>
			</widget>
	
	5 Maybe you want the SEARCH form on Nuxeo to use your own contentView layout
	
		<extension target="org.nuxeo.ecm.platform.actions.ActionService"
			point="actions">
			<action id="Invoice_Search_search_action" enabled="true" order="0"
				type="link" immediate="false" link=".">
				<category>SEARCH_CONTENT_VIEWS</category>
				<properties>
					<property name="contentViewName">Invoice_Search</property>
				</properties>
			</action>
		</extension>
	