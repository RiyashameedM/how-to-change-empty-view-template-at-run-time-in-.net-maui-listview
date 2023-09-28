# how-to-change-empty-view-template-at-run-time-in-.net-maui-listview

This demo shows about how to change the EmptyViewTemplate in .NET MAUI ListView

## XAML 
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:ListViewBoolToSortImageConverter x:Key="BoolToSortIconConverter"/>          
            <DataTemplate x:Key="BasicTemplate">
                <Label Text="{Binding .,StringFormat='{0} is not found'}" HorizontalTextAlignment="Center" VerticalOptions="CenterAndExpand"
                       FontSize="18" FontFamily="Roboto-Regular" TextColor="#666666" Margin="16,0,16,0"/>
            </DataTemplate>
            <DataTemplate  x:Key="AdvancedTemplate">
                <StackLayout VerticalOptions="CenterAndExpand">
                    <Label Text="&#xe725;" FontSize="40" TextColor="#666666" Opacity="0.8"
                                   FontFamily="{OnPlatform iOS=ListViewFontIcons, MacCatalyst=ListViewFontIcons, Android=ListViewFontIcons.ttf#, UWP=ListViewFontIcons.ttf#ListViewFontIcons}"
                                   HorizontalTextAlignment="Center" VerticalTextAlignment="Center" />
                    <Label Text="{Binding .,StringFormat='{0} is not found'}" TextColor="#666666" FontSize="16" FontFamily="Roboto-Regular" HorizontalTextAlignment="Center" VerticalTextAlignment="Center"  Margin="0,10,0,0"/>
                </StackLayout>
            </DataTemplate>
            <local:EmptyViewDataTemplateSelector x:Key="DataTemplateSelector" BasicTemplate="{StaticResource BasicTemplate}" AdvancedTemplate="{StaticResource AdvancedTemplate}"/>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.BindingContext>
        <local:SortingFilteringViewModel x:Name="viewModel"/>
    </ContentPage.BindingContext>

    <ContentPage.Content>
        <Grid Margin="0">
            <Grid.RowDefinitions>
                <RowDefinition Height="30"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="30"/>
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
                ---
            <syncfusion:SfListView x:Name="listView" 
                       Grid.Row="3"
                       SelectionMode="None"                                     
                       ItemsSource="{Binding Items}"                      
                       ItemSize="56"
                       EmptyView="{Binding Source={x:Reference filterText},Path=Text}"
                       EmptyViewTemplate="{StaticResource DataTemplateSelector}">

                <syncfusion:SfListView.ItemTemplate>
                    ---
                </syncfusion:SfListView.ItemTemplate>
            </syncfusion:SfListView>
## C#

    public class EmptyViewDataTemplateSelector : Microsoft.Maui.Controls.DataTemplateSelector
    {
        public DataTemplate BasicTemplate { get; set; }
        public DataTemplate AdvancedTemplate { get; set; }

        public EmptyViewDataTemplateSelector()
        {
         
        }

        protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
        {
            if(item.ToString().Count() > 10)
                return AdvancedTemplate;
            else
                return BasicTemplate;
        }
    }

## Requirements to run the demo

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) or [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)
* Xamarin add-ons for Visual Studio (available via the Visual Studio installer).

## Troubleshooting

### Path too long exception

If you are facing path too long exception when building this example project, close Visual Studio and rename the repository to short and build the project.