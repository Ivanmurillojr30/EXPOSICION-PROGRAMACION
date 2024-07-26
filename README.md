# EXPOSICION-PROGRAMACION

#MENU DE LOS RESULTADOS 
![Captura de pantalla 2024-07-25 215235](https://github.com/user-attachments/assets/bef1cf73-1088-4354-9e48-4e3bf10fb47b)

![Captura de pantalla 2024-07-25 215245](https://github.com/user-attachments/assets/c973ae08-d611-4d78-9295-1b38b0246c7d)

![Captura de pantalla 2024-07-25 215257](https://github.com/user-attachments/assets/6c46bc1a-6ac2-4cf4-ba9c-b989e6e4f2ce)


# Visor de Resultados de Constructores

#EJECUCION 

#EJECUCION 1

![Captura de pantalla 2024-07-11 215051](https://github.com/user-attachments/assets/b0580256-4ef7-4955-88d8-a2cd4618d3e1)

#EJECUCION 2

![Captura de pantalla 2024-07-11 215109](https://github.com/user-attachments/assets/8587fb7b-75a2-44c1-a6bf-db00381f6055)

#EJECUCION 3

![Captura de pantalla 2024-07-11 215100](https://github.com/user-attachments/assets/747a7e75-4c80-4b93-b703-921dd797f3ab)


# ESTRUCTURA DEL PROYECTO 

![image](https://github.com/user-attachments/assets/d33d49e3-62dd-472d-a166-0beaa133342c)



Esta aplicación JavaFX permite a los usuarios ver los resultados de los constructores por año. La aplicación recupera datos de una base de datos y los muestra en una tabla. Los usuarios pueden seleccionar un año del menú desplegable para filtrar los resultados.

## Características

- Selección de un año para ver los resultados de los constructores.
- Mostrar nombre del constructor, victorias, puntos totales y rango en una tabla.
- Actualización dinámica de la tabla según el año seleccionado.

## Requisitos Previos

Antes de comenzar, asegúrate de cumplir con los siguientes requisitos:

- Java Development Kit (JDK) 8 o posterior.
- JavaFX SDK.
- Una base de datos con las tablas y datos necesarios.
- Controlador JDBC para tu base de datos (por ejemplo, MySQL, PostgreSQL).

# CODIGO MAIN.JAVA

package demo_jdbc;

import java.util.List;
import java.util.stream.Collectors;

import demo_jdbc.models.Constructors;
import demo_jdbc.respositories.ConstructorsRepository;

import demo_jdbc.models.Season;
import demo_jdbc.respositories.SeasonRepository;

import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.Scene;
import javafx.scene.control.ComboBox;
import javafx.scene.control.TableColumn;
import javafx.scene.control.TableView;
import javafx.scene.control.cell.PropertyValueFactory;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class Main extends Application {

    @Override
    public void start(Stage primaryStage) {
        ComboBox<String> yearComboBox = createComboBox();

        TableView<Constructors> tableView = new TableView<>();

        TableColumn<Constructors, String> nameColumn = new TableColumn<>("Constructor Name");
        nameColumn.setCellValueFactory(new PropertyValueFactory<>("name"));

        TableColumn<Constructors, Integer> winsColumn = new TableColumn<>("Wins");
        winsColumn.setCellValueFactory(new PropertyValueFactory<>("wins"));

        TableColumn<Constructors, Integer> totalPointsColumn = new TableColumn<>("Total Points");
        totalPointsColumn.setCellValueFactory(new PropertyValueFactory<>("totalPoints"));

        TableColumn<Constructors, Integer> rankColumn = new TableColumn<>("Rank");
        rankColumn.setCellValueFactory(new PropertyValueFactory<>("seasonRank"));

        tableView.getColumns().add(nameColumn);
        tableView.getColumns().add(winsColumn);
        tableView.getColumns().add(totalPointsColumn);
        tableView.getColumns().add(rankColumn);

        yearComboBox.valueProperty().addListener((observable, oldValue, newValue) -> {
            if (newValue != null) {
                try {
                    int selectedYear = Integer.parseInt(newValue);
                    ConstructorsRepository constructorsRepository = new ConstructorsRepository();
                    List<Constructors> results = constructorsRepository.getResultByYear(selectedYear);
                    if (results != null) {
                        tableView.setItems(FXCollections.observableArrayList(results));
                    } else {
                        System.out.println("No se encontraron resultados para el año " + selectedYear);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        });

        VBox vbox = new VBox(yearComboBox, tableView);
        Scene scene = new Scene(vbox, 400, 400);

        primaryStage.setScene(scene);
        primaryStage.setTitle("Constructor Results");
        primaryStage.show();
    }

    public ComboBox<String> createComboBox() {
        ComboBox<String> comboBox = new ComboBox<>();
        comboBox.setPrefSize(100, 10);

        SeasonRepository seasonRepository = new SeasonRepository();
        List<Season> years = seasonRepository.getSeasons().stream()
                .sorted((s1, s2) -> Integer.compare(s1.getYear(), s2.getYear()))
                .collect(Collectors.toList());

        for (Season season : years) {
            comboBox.getItems().add(String.valueOf(season.getYear()));
        }

        comboBox.valueProperty().addListener((observable, oldValue, newValue) -> {
            System.out.println("Ha seleccionado el año: " + newValue);
        });

        return comboBox;
    }

    public static void main(String[] args) {
        launch(args);
    }
}

## Conclusión

Este proyecto de Visor de Resultados de Constructores demuestra cómo se puede integrar JavaFX con JDBC para crear una interfaz gráfica que permite a los usuarios interactuar con datos de una base de datos de forma intuitiva. Al seleccionar un año específico, la aplicación muestra los resultados de los constructores correspondientes, facilitando el análisis y visualización de los datos históricos.

Esperamos que esta aplicación sea útil para aprender y comprender cómo construir aplicaciones de escritorio en Java utilizando JavaFX y JDBC. Las contribuciones y sugerencias para mejorar el proyecto son siempre bienvenidas. ¡Gracias por utilizar nuestra aplicación!


