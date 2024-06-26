# 001 - Criar um aplicativo multiplataforma com o .NET MAUI


## App.xaml

Esse arquivo define os recursos do aplicativo que o aplicativo usará no layout XAML. Os recursos padrão estão localizados 
na pasta Resources e definem as cores em todo o aplicativo e os estilos padrão para cada controle interno do .NET MAUI. 

```
<?xml version = "1.0" encoding = "UTF-8" ?>
<Application xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MyMauiApp"
             x:Class="MyMauiApp.App">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Resources/Colors.xaml" />
                <ResourceDictionary Source="Resources/Styles.xaml" />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

<br/>

## App.xaml.cs

Esse é o code-behind do arquivo App.xaml. Esse arquivo define a classe App. Essa classe representa seu aplicativo no runtime. 
O construtor nessa classe cria uma janela inicial e a atribui para a propriedade MainPage; essa propriedade determina qual página é exibida quando 
o aplicativo começa a ser executado. Além disso, essa classe permite substituir manipuladores comuns de eventos de ciclo de vida de aplicativos com 
neutralidade de plataforma. Os eventos incluem OnStart, OnResume e OnSleep. Esses manipuladores são definidos como membros da classe base Application.


<br/>
Observação:
<br/>

Você também pode substituir eventos de ciclo de vida específicos da plataforma quando o aplicativo começar a ser executado pela primeira vez. Isso é descrito posteriormente.

```
namespace MyMauiApp;

public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        MainPage = new AppShell();
    }

    protected override void OnStart()
    {
        base.OnStart();
    }

    protected override void OnResume()
    {
        base.OnResume();
    }

    protected override void OnSleep()
    {
        base.OnSleep();
    }
}
```


<br/>

## AppShell.xaml

Esse arquivo é a estrutura principal de um aplicativo .NET MAUI. 
O Shell do .NET MAUI fornece muitos recursos que são benéficos para aplicativos de várias plataformas, 
incluindo estilo de aplicativo, navegação baseada em URI e opções de layout, incluindo navegação de submenu e guias 
para a raiz do aplicativo. O modelo padrão fornece uma página (ou ShellContent) que é carregada quando o aplicativo é iniciado.         

```
<?xml version="1.0" encoding="UTF-8" ?>
  <Shell
      x:Class="MyMauiApp.AppShell"
      xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
      xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
      xmlns:local="clr-namespace:MyMauiApp"
      Shell.FlyoutBehavior="Disabled">

      <ShellContent
          Title="Home"
          ContentTemplate="{DataTemplate local:MainPage}"
          Route="MainPage" />

  </Shell>
```

<br/>

## MainPage.xaml

Esse arquivo contém a definição da interface do usuário. 
O aplicativo de exemplo que o modelo de aplicativo MAUI gera contém dois rótulos, um botão e uma imagem. Os controles são organizados 
usando um VerticalStackLayout delimitado em um ScrollView. O controle VerticalStackLayout organiza os controles verticalmente (em uma pilha) 
e o ScrollView fornece uma barra de rolagem se a exibição é muito grande para ser exibida no dispositivo. Você deve substituir o conteúdo 
desse arquivo pelo seu layout de interface do usuário. Você também poderá definir mais páginas XAML se tiver um aplicativo de várias páginas.

```
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MyMauiApp.MainPage">

    <ScrollView>
        <VerticalStackLayout 
            Spacing="25" 
            Padding="30,0" 
            VerticalOptions="Center">

            <Image
                Source="dotnet_bot.png"
                SemanticProperties.Description="Cute dot net bot waving hi to you!"
                HeightRequest="200"
                HorizontalOptions="Center" />

            <Label 
                Text="Hello, World!"
                SemanticProperties.HeadingLevel="Level1"
                FontSize="32"
                HorizontalOptions="Center" />

            <Label 
                Text="Welcome to .NET Multi-platform App UI"
                SemanticProperties.HeadingLevel="Level2"
                SemanticProperties.Description="Welcome to dot net Multi platform App U I"
                FontSize="18"
                HorizontalOptions="Center" />

            <Button 
                x:Name="CounterBtn"
                Text="Click me"
                SemanticProperties.Hint="Counts the number of times you click"
                Clicked="OnCounterClicked"
                HorizontalOptions="Center" />

        </VerticalStackLayout>
    </ScrollView>

</ContentPage>
```

<br/>

## MainPage.xaml.cs

Esse é o code-behind da página. Nesse arquivo, você define a lógica dos vários manipuladores de eventos e outras ações 
disparadas pelos controles na página. O código de exemplo implementa um manipulador do evento Clicked para o botão na página. O código 
simplesmente incrementa uma variável de contador e exibe o resultado em uma etiqueta na página. O serviço semântico fornecido como parte da 
biblioteca do MAUI Essentials dá suporte à acessibilidade. O método estático Announce da classe SemanticScreenReader especifica o texto anunciado 
por um leitor de tela quando o usuário seleciona o botão:

```
namespace MyMauiApp;

public partial class MainPage : ContentPage
{
    int count = 0;

    public MainPage()
    {
        InitializeComponent();
    }

    private void OnCounterClicked(object sender, EventArgs e)
    {
        count++;

        if (count == 1)
            CounterBtn.Text = $"Clicked {count} time";
        else
            CounterBtn.Text = $"Clicked {count} times";

        SemanticScreenReader.Announce(CounterBtn.Text);
    }
}
```

<br/>

## MauiProgram.cs

Cada plataforma nativa tem um ponto de partida diferente que cria e inicializa o aplicativo. Você pode encontrar 
esse código na pasta Platforms no projeto. Esse código é específico da plataforma, mas no final ele chama o método CreateMauiApp 
da classe estática MauiProgram. Use o método CreateMauiApp para configurar o aplicativo criando um objeto do construtor de aplicativos. 
No mínimo, você precisa especificar qual classe descreve seu aplicativo. Você faz isso com o método genérico UseMauiApp do objeto do 
construtor de aplicativos; o parâmetro de tipo especifica a classe de aplicativo. O construtor de aplicativos também fornece métodos 
para tarefas como registrar fontes, configurar serviços para injeção de dependência, registrar manipuladores personalizados para controles 
e muito mais. O seguinte código mostra um exemplo de como usar o construtor de aplicativos para registrar uma fonte:

```
namespace MyMauiApp;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            .UseMauiApp<App>()
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
            });

        return builder.Build();
    }
}
```

<br/>

## Pasta - Platforms

Essa pasta contém recursos e arquivos de código de inicialização específicos da plataforma. Há pastas para Android, 
iOS, MacCatalyst, Tizen e Windows. No runtime, o aplicativo é iniciado de uma forma específica para a plataforma. 
Grande parte do processo de inicialização é abstraído pelos componentes internos das bibliotecas MAUI, mas os arquivos de 
código nessas pastas fornecem um mecanismo para conectar sua inicialização personalizada. O ponto importante é que, quando 
a inicialização é concluída, o código específico da plataforma chama o método MauiProgram.CreateMauiApp, que cria e executa 
o objeto App conforme descrito anteriormente. Por exemplo, o arquivo MainApplication.cs na pasta Android, o arquivo AppDelegate.cs 
na pasta iOS e MacCatalyst, o arquivo App.xaml.cs na pasta Windows contêm todas as substituições

```
protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();
```

<br/>

## Recursos do projeto

O arquivo .csproj do projeto principal inclui várias seções em destaque. O PropertyGroup inicial 
especifica as estruturas de plataforma às quais o projeto se destina, bem como itens como o título do aplicativo, 
a ID, a versão, a versão de exibição e os sistemas operacionais com suporte. Você pode corrigir essas propriedades 
conforme necessário.

```
<Project Sdk="Microsoft.NET.Sdk">

    <PropertyGroup>
        <TargetFrameworks>net6.0-android;net6.0-ios;net6.0-maccatalyst</TargetFrameworks>
        <TargetFrameworks Condition="$([MSBuild]::IsOSPlatform('windows'))">$(TargetFrameworks);net6.0-windows10.0.19041.0</TargetFrameworks>
        <!-- Uncomment to also build the tizen app. You will need to install tizen by following this: https://github.com/Samsung/Tizen.NET -->
        <!-- <TargetFrameworks>$(TargetFrameworks);net6.0-tizen</TargetFrameworks> -->
        <OutputType>Exe</OutputType>
        <RootNamespace>MyMauiApp</RootNamespace>
        <UseMaui>true</UseMaui>
        <SingleProject>true</SingleProject>
        <ImplicitUsings>enable</ImplicitUsings>

        <!-- Display name -->
        <ApplicationTitle>MyMauiApp</ApplicationTitle>

        <!-- App Identifier -->
        <ApplicationId>com.companyname.mymauiapp</ApplicationId>
        <ApplicationIdGuid>272B9ECE-E038-4E53-8553-E3C9EA05A5B2</ApplicationIdGuid>

        <!-- Versions -->
        <ApplicationDisplayVersion>1.0</ApplicationDisplayVersion>
        <ApplicationVersion>1</ApplicationVersion>

        <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'ios'">14.2</SupportedOSPlatformVersion>
        <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'maccatalyst'">14.0</SupportedOSPlatformVersion>
        <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'android'">21.0</SupportedOSPlatformVersion>
        <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'windows'">10.0.17763.0</SupportedOSPlatformVersion>
        <TargetPlatformMinVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'windows'">10.0.17763.0</TargetPlatformMinVersion>
        <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'tizen'">6.5</SupportedOSPlatformVersion>
    </PropertyGroup>
    ...

</Project>
```

A seção ItemGroup abaixo do grupo de propriedades inicial possibilita que você especifique uma imagem e uma cor para 
a tela inicial que aparece enquanto o aplicativo está sendo carregado, antes da primeira janela aparecer. 
Você também pode definir os locais padrão para AS fontes, imagens e ativos que o aplicativo usa.

```
<Project Sdk="Microsoft.NET.Sdk">

    ...

   <ItemGroup>
        <!-- App Icon -->
        <MauiIcon Include="Resources\appicon.svg" 
                  ForegroundFile="Resources\appiconfg.svg" 
                  Color="#512BD4" />

        <!-- Splash Screen -->
        <MauiSplashScreen Include="Resources\appiconfg.svg" 
                          Color="#512BD4" 
                          BaseSize="128,128" />

        <!-- Images -->
        <MauiImage Include="Resources\Images\*" />
        <MauiImage Update="Resources\Images\dotnet_bot.svg" 
                   BaseSize="168,208" />

        <!-- Custom Fonts -->
        <MauiFont Include="Resources\Fonts\*" />

        <!-- Raw Assets (also remove the "Resources\Raw" prefix) -->
        <MauiAsset Include="Resources\Raw\**" 
                   LogicalName="%(RecursiveDir)%(Filename)%(Extension)" />
    </ItemGroup>

    ...

</Project>
```

Na janela Gerenciador de Soluções no Visual Studio, você pode expandir a pasta Resources para ver esses itens. Você pode adicionar qualquer outra fonte, imagens e outros recursos gráficos exigidos pelo aplicativo a essa pasta e subpastas.

<br />

Você deverá registrar todas as fontes adicionadas à pasta de fontes com o objeto do construtor de aplicativos quando o aplicativo começar a ser executado. Lembre-se de que o método CreateMauiApp na classe MauiProgram faz isso com o método ConfigureFonts:

```
public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        builder
            ...
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
            });

        ...
    }
}
```

<br />

Neste exemplo, o método AddFont associa a fonte ao nome OpenSansRegular. Você pode especificar essa fonte ao formatar itens na descrição XAML de uma página ou no dicionário de recursos do aplicativo:

```
<Application ...">
    <Application.Resources>
        <ResourceDictionary>
            ...
            <Style TargetType="Button">
                ...
                <Setter Property="FontFamily" Value="OpenSansRegular" />
                ...
            </Style>

        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Use as pastas Resources no Android e as pastas iOS na pasta Platforms para recursos específicos da plataforma Android e iOS.
