# Load packages ----------------------------------------------------------------

library(shiny)
library(ggplot2)
library(tools)
library(shinythemes)

# Load data --------------------------------------------------------------------

load("movies.RData")

# Source utils.R ---------------------------------------------------------------
source("utils.R")

# Define UI --------------------------------------------------------------------

ui <- shiny::fluidPage(theme = shinythemes::shinytheme("spacelab"),
  shiny::sidebarLayout(
    shiny::sidebarPanel(
      shiny::selectInput(
        inputId = "y",
        label = "Y-axis:",
        choices = c(
          "IMDB rating" = "imdb_rating",
          "IMDB number of votes" = "imdb_num_votes",
          "Critics Score" = "critics_score",
          "Audience Score" = "audience_score",
          "Runtime" = "runtime"
        ),
        selected = "audience_score"
      ),

      shiny::selectInput(
        inputId = "x",
        label = "X-axis:",
        choices = c(
          "IMDB rating" = "imdb_rating",
          "IMDB number of votes" = "imdb_num_votes",
          "Critics Score" = "critics_score",
          "Audience Score" = "audience_score",
          "Runtime" = "runtime"
        ),
        selected = "critics_score"
      ),

      shiny::selectInput(
        inputId = "z",
        label = "Color by:",
        choices = c(
          "Title Type" = "title_type",
          "Genre" = "genre",
          "MPAA Rating" = "mpaa_rating",
          "Critics Rating" = "critics_rating",
          "Audience Rating" = "audience_rating"
        ),
        selected = "mpaa_rating"
      ),

      shiny::sliderInput(
        inputId = "alpha",
        label = "Alpha:",
        min = 0, max = 1,
        value = 0.4
      ),

      shiny::sliderInput(
        inputId = "size",
        label = "Size:",
        min = 0, max = 5,
        value = 3
      ),

      shiny::textInput(
        inputId = "plot_title",
        label = "Plot title",
        placeholder = "Enter text to be used as plot title"
      ),

      shiny::actionButton(
        inputId = "update_plot_title",
        label = "Update plot title"
      )
    ),

    shiny::mainPanel(
      shiny::br(),
      shiny::p(
        "These data were obtained from",
        shiny::a("IMBD", href = "http://www.imbd.com/"), "and",
        shiny::a("Rotten Tomatoes", href = "https://www.rottentomatoes.com/"), "."
      ),
      shiny::p("The data represent", 
        nrow(movies), 
        "randomly sampled movies released between 1972 to 2014 in the United States."),
      shiny::plotOutput(outputId = "scatterplot"),
      shiny::hr(),
        shiny::p(shiny::em("The code for this shiny application comes from", 
          shiny::a("Building Web Applications with shiny", 
            href = "https://rstudio-education.github.io/shiny-course/")
        ))
    )
  )
)

# Define server ----------------------------------------------------------------

server <- function(input, output, session) {
  
  new_plot_title <- shiny::reactive({
      toTitleCase(input$plot_title)
    }) |> 
    shiny::bindEvent(input$update_plot_title, 
                     ignoreNULL = FALSE, 
                     ignoreInit = FALSE)
    

  output$scatterplot <- shiny::renderPlot({
    point_plot(
        df = movies,
        x_var = input$x,
        y_var = input$y,
        col_var = input$z,
        alpha_var = input$alpha,
        size_var = input$size
      ) + 
      ggplot2::labs(title = new_plot_title()) + 
      ggplot2::theme_minimal() +
      ggplot2::theme(legend.position = "bottom")
  })
}

# Create the Shiny app object --------------------------------------------------

shiny::shinyApp(ui = ui, server = server)