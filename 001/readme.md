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

<br />

> Passo 5: Adicionar controles visuais a um aplicativo .NET MAUI

Agora que você usou o modelo do .NET MAUI para criar o aplicativo, a próxima etapa é adicionar a interface do usuário e implementar a lógica de interface do usuário inicial.
<br /><br />
Nesta unidade, você aprenderá mais sobre os blocos de construção de um aplicativo .NET MAUI e das estruturas de navegação.

## O que há em um projeto do .NET MAUI?

Para recapitular, um projeto do .NET MAUI contém inicialmente:

- O arquivo MauiProgram.cs que contém o código para criar e configurar o objeto Application.
- Os arquivos App.xaml e App.xaml.cs que fornecem recursos de interface do usuário e criam a janela inicial para o aplicativo.
- Os arquivos AppShell.xaml e AppShell.xaml.cs que especificam a página inicial do aplicativo e manipulam o registro de páginas para roteamento de navegação.
- Os arquivos MainPage.xaml e MainPage.xaml.cs que definem o layout e a lógica da interface do usuário para a página exibida por padrão na janela inicial.

Você pode adicionar mais páginas ao aplicativo conforme necessário e pode criar classes adicionais para implementar a lógica de negócios exigida pelo aplicativo.
<br /><br />
Um projeto do .NET MAUI também contém um conjunto padrão de recursos de aplicativo, como imagens, ícones e fontes, e o código de inicialização padrão para cada plataforma.

## A classe do aplicativo

A classe App representa o aplicativo .NET MAUI como um todo. Ela herda um conjunto padrão de comportamentos de Microsoft.Maui.Controls.Application. Lembre-se de que, na unidade anterior, é a classe App que é instanciada e carregada pelo código de inicialização para cada plataforma. O construtor de classe App, por sua vez, geralmente criará uma instância da classe AppShell e a atribuirá à propriedade MainPage. Esse é o código que controla a primeira tela que o usuário vê por meio do que está definido em AppShell.
<br /><br />
A classe App também contém:

- Métodos para lidar com eventos de ciclo de vida, incluindo quando o aplicativo é enviado para o segundo plano (ou seja, quando ele deixa de ser o aplicativo em primeiro plano).
- Métodos para criar Windows para o aplicativo. O aplicativo .NET MAUI tem uma só janela por padrão, mas você pode criar e iniciar janelas adicionais, o que é útil em aplicativos para desktop e tablet.

## Shell

O Shell do .NET MAUI (NET Multi-platform App UI) reduz a complexidade do desenvolvimento de aplicativos fornecendo os recursos fundamentais que a maioria dos aplicativos exige, incluindo:

- Um só local para descrever a hierarquia visual de um aplicativo.
- Uma experiência de navegação comum para o usuário.
- Um esquema de navegação baseado em URI que permite a navegação para qualquer página no aplicativo.
- Um manipulador de pesquisa integrado.

Em um aplicativo de Shell do .NET MAUI, a hierarquia visual do aplicativo é descrita em uma classe funciona como uma subclasse da classe Shell. Essa classe pode ser composta por três objetos hierárquicos principais:

- FlyoutItem ou TabBar. Um FlyoutItem representa um ou mais itens no submenu e deve ser usado quando o padrão de navegação do aplicativo requer um submenu. Uma TabBar representa a barra de guias inferior e deve ser usada quando o padrão de navegação para o aplicativo começa com guias inferiores e não exige um submenu.
- Tab, que representa o conteúdo agrupado, navegável pelas guias inferiores.
- ShellContent, que representa os objetos ContentPage para cada guia.

Esses objetos não representam nenhuma interface do usuário, e sim a organização da hierarquia visual do aplicativo. O Shell usa esses objetos e produz a interface do usuário de navegação para o conteúdo.

## Pages (Páginas)

As páginas são a raiz da hierarquia da interface do usuário no .NET MAUI dentro de um Shell. A solução que você viu até agora inclui uma classe chamada MainPage. Essa classe deriva de ContentPage, que é o tipo de página mais simples e mais comum. Uma página de conteúdo simplesmente exibe seu conteúdo. O .NET MAUI tem vários outros tipos de páginas internas, incluindo os seguintes:

- TabbedPage: Esta é a página raiz usada para navegação em guias. Uma página com guias contém objetos de página filho; um para cada guia.
- FlyoutPage: esta página permite que você implemente uma apresentação no estilo mestre/detalhes. Uma página de submenu contém uma lista de itens. Quando você seleciona um item, aparece uma exibição que mostra os detalhes desse item.

Outros tipos de página estão disponíveis e são usados principalmente para habilitar padrões de navegação diferentes em aplicativos com várias telas.

## Exibições

Uma página de conteúdo normalmente mostra uma exibição. Uma exibição permite recuperar e apresentar dados de uma maneira específica. A exibição padrão de uma página de conteúdo é um ContentView, que exibe os itens no estado em que se encontram. Se reduzir a exibição, os itens poderão desaparecer até que você redimensione a exibição. Um ScrollView permite exibir itens em uma janela de rolagem. Se você reduzir a janela, poderá rolar para cima e para baixo para exibir os itens. Um CarouselView é uma exibição rolável que permite que o usuário passe o dedo para percorrer uma coleção de itens. Um CollectionView pode recuperar dados de uma fonte de dados nomeada e apresentar cada item usando um modelo como formato. Há muitos outros tipos de exibições disponíveis.

## Controles e layouts

Uma exibição pode conter apenas um controle, como um botão, um rótulo ou caixas de texto. (A rigor, uma exibição é em si um controle, portanto, uma exibição pode conter outra exibição.) Entretanto, uma interface do usuário restrita a um só controle não seria muito útil, por isso os controles são posicionados em um layout. Um layout define as regras segundo as quais os controles são exibidos em relação uns aos outros. Um layout também é um controle, portanto você pode adicioná-lo a uma exibição. Se você examinar o arquivo MainPage.xaml padrão, verá essa hierarquia de página/exibição/layout/controle em ação. Nesse código XAML, o elemento VerticalStackLayout é apenas um controle que permite ajustar o layout de outros controles.

```
<ContentPage ...>
    <ScrollView ...>
        <VerticalStackLayout>
            <Image ... />
            <Label ... />
            <Label ... />
            <Button ... />
        </VerticalStackLayout>
    </ScrollView>
</ContentPage>
```

Alguns dos controles comuns usados para definir layouts são:

- VerticalStackLayout e HorizontalStackLayout, que são layouts de pilha otimizados que dispõem os controles em uma pilha de cima para baixo ou da esquerda para a direita. Um StackLayout também está disponível, com uma propriedade chamada StackOrientation que você pode definir como Horizontal ou Vertical. Em um tablet ou telefone, modificar essa propriedade no código do aplicativo permite ajustar a exibição caso o usuário gire o dispositivo
- AbsoluteLayout, que permite definir coordenadas exatas para os controles.
- FlexLayout, que é semelhante a StackLayout, mas permite encapsular os controles filho que ele contém se eles não couberem em uma só linha ou coluna. Esse layout também fornece opções de alinhamento e adaptação a diferentes tamanhos de tela. Por exemplo, um controle FlexLayout pode alinhar seu controle filho à esquerda, à direita ou ao centro quando organizado verticalmente. Quando alinhados horizontalmente, você pode justificar os controles para garantir um espaçamento uniforme. Você pode usar um FlexLayout horizontal dentro de um ScrollView para exibir uma série de quadros roláveis horizontalmente (cada quadro pode ser um FlexLayout organizado verticalmente)
- Grid, que dispõe os controles conforme uma localização de linha e coluna que definimos. Você pode definir os tamanhos de linha e coluna, bem como as amplitudes, de modo que os layouts de grade não precisam necessariamente ter uma "aparência de xadrez".

Todos os controles têm propriedades. Você pode definir valores iniciais para essas propriedades usando XAML. Em muitos casos, você pode modificar essas propriedades no código C# do aplicativo. Por exemplo, o código que manipula o evento Clicked para o botão Clique em mim no aplicativo .NET MAUI padrão tem esta aparência:

```
int count = 0;
private void OnCounterClicked(object sender, EventArgs e)
{
    count+=5;

    if (count == 1)
        CounterBtn.Text = $"Clicked {count} time";
    else
        CounterBtn.Text = $"Clicked {count} times";

    SemanticScreenReader.Announce(CounterBtn.Text);
}
```

Esse código modifica a propriedade Text do controle CounterBtn na página. Você pode até mesmo criar páginas, exibições e layouts inteiros em seu código, sem precisar usar XAML. Por exemplo, considere a seguinte definição XAML de uma página:

```
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Phoneword.MainPage">

    <ScrollView>
        <VerticalStackLayout>
            <Label Text="Current count: 0"
                Grid.Row="0"
                FontSize="18"
                FontAttributes="Bold"
                x:Name="CounterLabel"
                HorizontalOptions="Center" />

            <Button Text="Click me"
                Grid.Row="1"
                Clicked="OnCounterClicked"
                HorizontalOptions="Center" />
        </VerticalStackLayout>
    </ScrollView>
</ContentPage>
```

O código equivalente em C# tem esta aparência:

```
public partial class TestPage : ContentPage
{
    int count = 0;
    
    // Named Label - declared as a member of the class
    Label counterLabel;

    public TestPage()
    {       
        var myScrollView = new ScrollView();

        var myStackLayout = new VerticalStackLayout();
        myScrollView.Content = myStackLayout;

        counterLabel = new Label
        {
            Text = "Current count: 0",
            FontSize = 18,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };
        myStackLayout.Children.Add(counterLabel);
        
        var myButton = new Button
        {
            Text = "Click me",
            HorizontalOptions = LayoutOptions.Center
        };
        myStackLayout.Children.Add(myButton);

        myButton.Clicked += OnCounterClicked;

        this.Content = myScrollView;
    }

    private void OnCounterClicked(object sender, EventArgs e)
    {
        count++;
        counterLabel.Text = $"Current count: {count}";

        SemanticScreenReader.Announce(counterLabel.Text);
    }
}
```

O código C# é mais detalhado, mas fornece flexibilidade adicional que permite adaptar a interface do usuário dinamicamente.
<br /><br />
Para exibir esta página quando o aplicativo começar a ser executado, defina a classe TestPage em AppShell como a principal ShellContent:

```
<ShellContent
        Title="Home"
        ContentTemplate="{DataTemplate local:TestPage}"
        Route="TestPage" />
```

## Ajustando um layout

É útil adicionar algum espaço de fôlego ao redor de um controle. Cada controle tem uma propriedade Margin que os layouts respeitam. Você pode pensar em margem como o controle empurrando os outros para longe.
<br /><br />
Todos os layouts também têm uma propriedade Padding que impede que qualquer um de seus filhos se aproxime da borda do layout. Uma maneira de pensar nesse conceito é que todos os controles estão em uma caixa e essa caixa tem paredes acolchoadas.
<br /><br />
Outra configuração útil de espaço em branco é a propriedade Spacing de VerticalStackLayout ou HorizontalStackLayout. Esse é o espaço entre todos os filhos do layout. Isso se soma à própria margem do controle, de modo que o espaço em branco real seria a margem mais o espaçamento.