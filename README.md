{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "source": [
        "Importing the Dependencies"
      ],
      "metadata": {
        "id": "o693i-yuJ2vz"
      }
    },
    {
      "cell_type": "code",
      "execution_count": 1,
      "metadata": {
        "id": "kwNafJ79I2iL"
      },
      "outputs": [],
      "source": [
        " import numpy as np\n",
        " import pandas as pd\n",
        " import matplotlib.pyplot as plt\n",
        " import seaborn as sns\n",
        " from sklearn.cluster import KMeans"
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "Data Collection & Analysis\n",
        "\n",
        "\n"
      ],
      "metadata": {
        "id": "h-WzAU1GKphg"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# loading the data from csv file to a pandas DataFrame\n",
        "customer_data = pd.read_csv('/content/Mall_Customers.csv')"
      ],
      "metadata": {
        "id": "21sWqt7DOAhR"
      },
      "execution_count": 2,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "# first 5 rows in the dataframe\n",
        "customer_data.head()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "1o-7nsW_OyPj",
        "outputId": "0d5b19f6-f661-4479-943b-9a58763c2c37"
      },
      "execution_count": 3,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "   CustomerID  Gender  Age  Annual Income (k$)  Spending Score (1-100)\n",
              "0           1    Male   19                  15                      39\n",
              "1           2    Male   21                  15                      81\n",
              "2           3  Female   20                  16                       6\n",
              "3           4  Female   23                  16                      77\n",
              "4           5  Female   31                  17                      40"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-7bd5e05c-1b78-4d37-bffb-2c7668518135\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>CustomerID</th>\n",
              "      <th>Gender</th>\n",
              "      <th>Age</th>\n",
              "      <th>Annual Income (k$)</th>\n",
              "      <th>Spending Score (1-100)</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>1</td>\n",
              "      <td>Male</td>\n",
              "      <td>19</td>\n",
              "      <td>15</td>\n",
              "      <td>39</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>2</td>\n",
              "      <td>Male</td>\n",
              "      <td>21</td>\n",
              "      <td>15</td>\n",
              "      <td>81</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>3</td>\n",
              "      <td>Female</td>\n",
              "      <td>20</td>\n",
              "      <td>16</td>\n",
              "      <td>6</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>4</td>\n",
              "      <td>Female</td>\n",
              "      <td>23</td>\n",
              "      <td>16</td>\n",
              "      <td>77</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>5</td>\n",
              "      <td>Female</td>\n",
              "      <td>31</td>\n",
              "      <td>17</td>\n",
              "      <td>40</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-7bd5e05c-1b78-4d37-bffb-2c7668518135')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-7bd5e05c-1b78-4d37-bffb-2c7668518135 button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-7bd5e05c-1b78-4d37-bffb-2c7668518135');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "<div id=\"df-047b2f49-3507-436f-9be9-8b9e361d4f35\">\n",
              "  <button class=\"colab-df-quickchart\" onclick=\"quickchart('df-047b2f49-3507-436f-9be9-8b9e361d4f35')\"\n",
              "            title=\"Suggest charts\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "<svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "     width=\"24px\">\n",
              "    <g>\n",
              "        <path d=\"M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z\"/>\n",
              "    </g>\n",
              "</svg>\n",
              "  </button>\n",
              "\n",
              "<style>\n",
              "  .colab-df-quickchart {\n",
              "      --bg-color: #E8F0FE;\n",
              "      --fill-color: #1967D2;\n",
              "      --hover-bg-color: #E2EBFA;\n",
              "      --hover-fill-color: #174EA6;\n",
              "      --disabled-fill-color: #AAA;\n",
              "      --disabled-bg-color: #DDD;\n",
              "  }\n",
              "\n",
              "  [theme=dark] .colab-df-quickchart {\n",
              "      --bg-color: #3B4455;\n",
              "      --fill-color: #D2E3FC;\n",
              "      --hover-bg-color: #434B5C;\n",
              "      --hover-fill-color: #FFFFFF;\n",
              "      --disabled-bg-color: #3B4455;\n",
              "      --disabled-fill-color: #666;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart {\n",
              "    background-color: var(--bg-color);\n",
              "    border: none;\n",
              "    border-radius: 50%;\n",
              "    cursor: pointer;\n",
              "    display: none;\n",
              "    fill: var(--fill-color);\n",
              "    height: 32px;\n",
              "    padding: 0;\n",
              "    width: 32px;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart:hover {\n",
              "    background-color: var(--hover-bg-color);\n",
              "    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "    fill: var(--button-hover-fill-color);\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart-complete:disabled,\n",
              "  .colab-df-quickchart-complete:disabled:hover {\n",
              "    background-color: var(--disabled-bg-color);\n",
              "    fill: var(--disabled-fill-color);\n",
              "    box-shadow: none;\n",
              "  }\n",
              "\n",
              "  .colab-df-spinner {\n",
              "    border: 2px solid var(--fill-color);\n",
              "    border-color: transparent;\n",
              "    border-bottom-color: var(--fill-color);\n",
              "    animation:\n",
              "      spin 1s steps(1) infinite;\n",
              "  }\n",
              "\n",
              "  @keyframes spin {\n",
              "    0% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "      border-left-color: var(--fill-color);\n",
              "    }\n",
              "    20% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    30% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    40% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    60% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    80% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "    90% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "  }\n",
              "</style>\n",
              "\n",
              "  <script>\n",
              "    async function quickchart(key) {\n",
              "      const quickchartButtonEl =\n",
              "        document.querySelector('#' + key + ' button');\n",
              "      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.\n",
              "      quickchartButtonEl.classList.add('colab-df-spinner');\n",
              "      try {\n",
              "        const charts = await google.colab.kernel.invokeFunction(\n",
              "            'suggestCharts', [key], {});\n",
              "      } catch (error) {\n",
              "        console.error('Error during call to suggestCharts:', error);\n",
              "      }\n",
              "      quickchartButtonEl.classList.remove('colab-df-spinner');\n",
              "      quickchartButtonEl.classList.add('colab-df-quickchart-complete');\n",
              "    }\n",
              "    (() => {\n",
              "      let quickchartButtonEl =\n",
              "        document.querySelector('#df-047b2f49-3507-436f-9be9-8b9e361d4f35 button');\n",
              "      quickchartButtonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "    })();\n",
              "  </script>\n",
              "</div>\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "customer_data",
              "summary": "{\n  \"name\": \"customer_data\",\n  \"rows\": 200,\n  \"fields\": [\n    {\n      \"column\": \"CustomerID\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 57,\n        \"min\": 1,\n        \"max\": 200,\n        \"num_unique_values\": 200,\n        \"samples\": [\n          96,\n          16,\n          31\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Gender\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 2,\n        \"samples\": [\n          \"Female\",\n          \"Male\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Age\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 13,\n        \"min\": 18,\n        \"max\": 70,\n        \"num_unique_values\": 51,\n        \"samples\": [\n          55,\n          26\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Annual Income (k$)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 26,\n        \"min\": 15,\n        \"max\": 137,\n        \"num_unique_values\": 64,\n        \"samples\": [\n          87,\n          101\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Spending Score (1-100)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 25,\n        \"min\": 1,\n        \"max\": 99,\n        \"num_unique_values\": 84,\n        \"samples\": [\n          83,\n          39\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 3
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# finding the number of rows and columns\n",
        "customer_data.shape"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "wlshJi0RPYWn",
        "outputId": "03b41dad-f9c5-4384-e58b-f3616f36a5a0"
      },
      "execution_count": 4,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "(200, 5)"
            ]
          },
          "metadata": {},
          "execution_count": 4
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# getting some information about the dataset\n",
        "customer_data.info()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "3OYsiRukPqBV",
        "outputId": "f3e95ff2-75e8-47f9-c4b6-27c01ffd168e"
      },
      "execution_count": 5,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 200 entries, 0 to 199\n",
            "Data columns (total 5 columns):\n",
            " #   Column                  Non-Null Count  Dtype \n",
            "---  ------                  --------------  ----- \n",
            " 0   CustomerID              200 non-null    int64 \n",
            " 1   Gender                  200 non-null    object\n",
            " 2   Age                     200 non-null    int64 \n",
            " 3   Annual Income (k$)      200 non-null    int64 \n",
            " 4   Spending Score (1-100)  200 non-null    int64 \n",
            "dtypes: int64(4), object(1)\n",
            "memory usage: 7.9+ KB\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# checking for missing values\n",
        "customer_data.isnull().sum()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "_-AozebrQF80",
        "outputId": "a7f168c7-a3e0-4f2e-d082-29290c559b9a"
      },
      "execution_count": 6,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "CustomerID                0\n",
              "Gender                    0\n",
              "Age                       0\n",
              "Annual Income (k$)        0\n",
              "Spending Score (1-100)    0\n",
              "dtype: int64"
            ]
          },
          "metadata": {},
          "execution_count": 6
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "Choosing the Annual Income Column & Spending Score column"
      ],
      "metadata": {
        "id": "amS7BOpm2H8p"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "X = customer_data.iloc[:,[3,4]].values"
      ],
      "metadata": {
        "id": "NiNzYUTwow8E"
      },
      "execution_count": 7,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "print(X)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "32zvJbVTtsZT",
        "outputId": "e3f98030-dec2-44fe-cbcf-54cdc1ba5266"
      },
      "execution_count": 8,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "[[ 15  39]\n",
            " [ 15  81]\n",
            " [ 16   6]\n",
            " [ 16  77]\n",
            " [ 17  40]\n",
            " [ 17  76]\n",
            " [ 18   6]\n",
            " [ 18  94]\n",
            " [ 19   3]\n",
            " [ 19  72]\n",
            " [ 19  14]\n",
            " [ 19  99]\n",
            " [ 20  15]\n",
            " [ 20  77]\n",
            " [ 20  13]\n",
            " [ 20  79]\n",
            " [ 21  35]\n",
            " [ 21  66]\n",
            " [ 23  29]\n",
            " [ 23  98]\n",
            " [ 24  35]\n",
            " [ 24  73]\n",
            " [ 25   5]\n",
            " [ 25  73]\n",
            " [ 28  14]\n",
            " [ 28  82]\n",
            " [ 28  32]\n",
            " [ 28  61]\n",
            " [ 29  31]\n",
            " [ 29  87]\n",
            " [ 30   4]\n",
            " [ 30  73]\n",
            " [ 33   4]\n",
            " [ 33  92]\n",
            " [ 33  14]\n",
            " [ 33  81]\n",
            " [ 34  17]\n",
            " [ 34  73]\n",
            " [ 37  26]\n",
            " [ 37  75]\n",
            " [ 38  35]\n",
            " [ 38  92]\n",
            " [ 39  36]\n",
            " [ 39  61]\n",
            " [ 39  28]\n",
            " [ 39  65]\n",
            " [ 40  55]\n",
            " [ 40  47]\n",
            " [ 40  42]\n",
            " [ 40  42]\n",
            " [ 42  52]\n",
            " [ 42  60]\n",
            " [ 43  54]\n",
            " [ 43  60]\n",
            " [ 43  45]\n",
            " [ 43  41]\n",
            " [ 44  50]\n",
            " [ 44  46]\n",
            " [ 46  51]\n",
            " [ 46  46]\n",
            " [ 46  56]\n",
            " [ 46  55]\n",
            " [ 47  52]\n",
            " [ 47  59]\n",
            " [ 48  51]\n",
            " [ 48  59]\n",
            " [ 48  50]\n",
            " [ 48  48]\n",
            " [ 48  59]\n",
            " [ 48  47]\n",
            " [ 49  55]\n",
            " [ 49  42]\n",
            " [ 50  49]\n",
            " [ 50  56]\n",
            " [ 54  47]\n",
            " [ 54  54]\n",
            " [ 54  53]\n",
            " [ 54  48]\n",
            " [ 54  52]\n",
            " [ 54  42]\n",
            " [ 54  51]\n",
            " [ 54  55]\n",
            " [ 54  41]\n",
            " [ 54  44]\n",
            " [ 54  57]\n",
            " [ 54  46]\n",
            " [ 57  58]\n",
            " [ 57  55]\n",
            " [ 58  60]\n",
            " [ 58  46]\n",
            " [ 59  55]\n",
            " [ 59  41]\n",
            " [ 60  49]\n",
            " [ 60  40]\n",
            " [ 60  42]\n",
            " [ 60  52]\n",
            " [ 60  47]\n",
            " [ 60  50]\n",
            " [ 61  42]\n",
            " [ 61  49]\n",
            " [ 62  41]\n",
            " [ 62  48]\n",
            " [ 62  59]\n",
            " [ 62  55]\n",
            " [ 62  56]\n",
            " [ 62  42]\n",
            " [ 63  50]\n",
            " [ 63  46]\n",
            " [ 63  43]\n",
            " [ 63  48]\n",
            " [ 63  52]\n",
            " [ 63  54]\n",
            " [ 64  42]\n",
            " [ 64  46]\n",
            " [ 65  48]\n",
            " [ 65  50]\n",
            " [ 65  43]\n",
            " [ 65  59]\n",
            " [ 67  43]\n",
            " [ 67  57]\n",
            " [ 67  56]\n",
            " [ 67  40]\n",
            " [ 69  58]\n",
            " [ 69  91]\n",
            " [ 70  29]\n",
            " [ 70  77]\n",
            " [ 71  35]\n",
            " [ 71  95]\n",
            " [ 71  11]\n",
            " [ 71  75]\n",
            " [ 71   9]\n",
            " [ 71  75]\n",
            " [ 72  34]\n",
            " [ 72  71]\n",
            " [ 73   5]\n",
            " [ 73  88]\n",
            " [ 73   7]\n",
            " [ 73  73]\n",
            " [ 74  10]\n",
            " [ 74  72]\n",
            " [ 75   5]\n",
            " [ 75  93]\n",
            " [ 76  40]\n",
            " [ 76  87]\n",
            " [ 77  12]\n",
            " [ 77  97]\n",
            " [ 77  36]\n",
            " [ 77  74]\n",
            " [ 78  22]\n",
            " [ 78  90]\n",
            " [ 78  17]\n",
            " [ 78  88]\n",
            " [ 78  20]\n",
            " [ 78  76]\n",
            " [ 78  16]\n",
            " [ 78  89]\n",
            " [ 78   1]\n",
            " [ 78  78]\n",
            " [ 78   1]\n",
            " [ 78  73]\n",
            " [ 79  35]\n",
            " [ 79  83]\n",
            " [ 81   5]\n",
            " [ 81  93]\n",
            " [ 85  26]\n",
            " [ 85  75]\n",
            " [ 86  20]\n",
            " [ 86  95]\n",
            " [ 87  27]\n",
            " [ 87  63]\n",
            " [ 87  13]\n",
            " [ 87  75]\n",
            " [ 87  10]\n",
            " [ 87  92]\n",
            " [ 88  13]\n",
            " [ 88  86]\n",
            " [ 88  15]\n",
            " [ 88  69]\n",
            " [ 93  14]\n",
            " [ 93  90]\n",
            " [ 97  32]\n",
            " [ 97  86]\n",
            " [ 98  15]\n",
            " [ 98  88]\n",
            " [ 99  39]\n",
            " [ 99  97]\n",
            " [101  24]\n",
            " [101  68]\n",
            " [103  17]\n",
            " [103  85]\n",
            " [103  23]\n",
            " [103  69]\n",
            " [113   8]\n",
            " [113  91]\n",
            " [120  16]\n",
            " [120  79]\n",
            " [126  28]\n",
            " [126  74]\n",
            " [137  18]\n",
            " [137  83]]\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "Choosing the number of clusters"
      ],
      "metadata": {
        "id": "EcYB6e7gt-Za"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "WCSS -> Within Clusters Sum of Squares"
      ],
      "metadata": {
        "id": "uTo6uO4rueQc"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# finding wcss value for different number of clusters\n",
        "\n",
        "wcss = []\n",
        "\n",
        "for i in range(1,11):\n",
        "  kmeans = KMeans(n_clusters=i, init='k-means++', random_state=42)\n",
        "  kmeans.fit(X)\n",
        "\n",
        "  wcss.append(kmeans.inertia_)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "collapsed": true,
        "id": "QGk4gDGSul10",
        "outputId": "9499f1b3-1475-44d3-abd9-6b96213a7670"
      },
      "execution_count": 9,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stderr",
          "text": [
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n",
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n",
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n",
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n",
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n",
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n",
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n",
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n",
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n",
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "# plot an elbow graph\n",
        "\n",
        "sns.set()\n",
        "plt.plot(range(1,11), wcss)\n",
        "plt.title('The Elbow Point Graph')\n",
        "plt.xlabel('Number of Clusters')\n",
        "plt.ylabel('WCSS')\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 480
        },
        "id": "skIW_3qHwImK",
        "outputId": "69688259-0379-4d0a-f8b2-be621fcd926e"
      },
      "execution_count": 10,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ],
            "image/png": "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rJCeno74+Hjk5ubed83bw4xBSUkJ+vbti8GDB6Nr166wtrbG77//jrNnzxpcRdm9e3fEx8dj8eLFCAgIgLW1NQYMGICRI0di165dePfdd3Hs2DGEhISguroaqamp2L17N1avXo2AgID7fs4FCxagqKgIffr0gYuLC7KysrB+/Xr4+fnBx8enXt8dkSlhECOiRuHp6YkRI0bgxx9/NHrun//8J/Lz87Fnzx7s2rULUVFRWL169T0XdT+q119/HZcuXcKqVatQUlKC8PBwvPvuu7CyshLahIWF4fvvv8dXX32F9evXo7S0FM7OzggMDMSYMWPq/d5t27bFd999h//85z9Yv349Kioq4OvrixUrVqBfv3717tfJyQnfffcdNm7ciPj4eHzxxRfQaDRwc3PDgAEDMGHChPu+/mHGwNLSEmPHjsXRo0exd+9e6HQ6eHh44N133zXYxPall15CcnIytm7dijVr1gg1SKVSLFu2DGvWrMG2bduwb98+WFlZoWPHjg99L0z9esONGzdCpVLB2dkZQ4cOxdSpU+sdYolMiURXn5WtRERERPTI+M8JIiIiIpEwiBERERGJhEGMiIiISCQMYkREREQiYRAjIiIiEgmDGBEREZFIGMSIiIiIRMIgRkRERCQSBjEiIiIikTCIEREREYmEQYyIiIhIJAxiRERERCL5//ZgoQZc0HanAAAAAElFTkSuQmCC\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "Optimum Number of Clusters = 5"
      ],
      "metadata": {
        "id": "yUmHrugm0OUq"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "Training the k-Means Clustering Model"
      ],
      "metadata": {
        "id": "QXEmjOip0XfW"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "kmeans = KMeans(n_clusters=5, init='k-means++', random_state=0)\n",
        "\n",
        "# return a label for each data point based on their cluster\n",
        "Y = kmeans.fit_predict(X)\n",
        "\n",
        "print(Y)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "aLfXTHHX0Vji",
        "outputId": "3ec68a90-a9ac-45ab-8f0a-ccf6c0148502"
      },
      "execution_count": 11,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "[4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4 3 4\n",
            " 3 4 3 4 3 4 1 4 3 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1\n",
            " 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1\n",
            " 1 1 1 1 1 1 1 1 1 1 1 1 2 0 2 1 2 0 2 0 2 1 2 0 2 0 2 0 2 0 2 1 2 0 2 0 2\n",
            " 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2 0\n",
            " 2 0 2 0 2 0 2 0 2 0 2 0 2 0 2]\n"
          ]
        },
        {
          "output_type": "stream",
          "name": "stderr",
          "text": [
            "/usr/local/lib/python3.10/dist-packages/sklearn/cluster/_kmeans.py:870: FutureWarning: The default value of `n_init` will change from 10 to 'auto' in 1.4. Set the value of `n_init` explicitly to suppress the warning\n",
            "  warnings.warn(\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "5 Clusters - 0,1,2,3,4"
      ],
      "metadata": {
        "id": "eOMIUy9eMF5T"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "Visualizing all the clusters"
      ],
      "metadata": {
        "id": "F-igmOaW0Uyf"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# plotting all the clusters and their Centroids\n",
        "\n",
        "plt.figure(figsize=(8,8))\n",
        "plt.scatter(X[Y==0,0], X[Y==0,1], s=50, c='green', label='cluster 1')\n",
        "plt.scatter(X[Y==1,0], X[Y==1,1], s=50, c='red', label='cluster 2')\n",
        "plt.scatter(X[Y==2,0], X[Y==2,1], s=50, c='yellow', label='cluster 3')\n",
        "plt.scatter(X[Y==3,0], X[Y==3,1], s=50, c='blue', label='cluster 4')\n",
        "plt.scatter(X[Y==4,0], X[Y==4,1], s=50, c='violet', label='cluster 5')\n",
        "\n",
        "# plot the centroids\n",
        "plt.scatter(kmeans.cluster_centers_[:,0], kmeans.cluster_centers_[:,1], s=100, c='cyan', label='Centroids')\n",
        "\n",
        "plt.title('Customer Groups')\n",
        "plt.xlabel('Annual Income')\n",
        "plt.ylabel('Spending Score')\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 727
        },
        "id": "94deGoLbq2Lv",
        "outputId": "89f3e515-8c08-46cc-d3f1-c349233bec84"
      },
      "execution_count": 12,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 800x800 with 1 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAAsEAAALGCAYAAACktEzMAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuMSwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/bCgiHAAAACXBIWXMAAA9hAAAPYQGoP6dpAACp2ElEQVR4nOzde3xT9f0/8NfnJE2b0hYoF6kKUvSnIijo/NriBRX9Slv4wph3na6KCipsAip8/c65i5vDr8KmVUFQ0HmZOm8Ihbp5wyHlO92Q4dymclVBVC5Nado0OZ/fH2lC07TJyck5J+ckr+cePliTk3M++aRt3v3k836/hZRSgoiIiIgohyiZHgARERERkdUYBBMRERFRzmEQTEREREQ5h0EwEREREeUcBsFERERElHMYBBMRERFRzmEQTEREREQ5h0EwEREREeUcBsFERERElHMYBBMRERFRznFnegBERFbZsWMHli5dinXr1mHPnj3Iy8vDsccei+rqalx66aUoKCgw/JqvvfYavv32W9TW1hp+brvYuXMnli1bhnXr1mH37t0AgCOOOAIVFRW49NJLcfzxx2d4hERE8YSUUmZ6EEREZnv77bfxox/9CB6PB5MnT8axxx6L9vZ2fPDBB3j99dcxZcoU/OIXvzD8utOmTcMnn3yCN9980/Bz28Fbb72FWbNmweVy4b/+679w/PHHQ1EUbNmyBa+//jq+/PJLvPHGGzjiiCMyPVQiohhcCSairLdz507MmjULhx9+OJ544gkMHDgwet+VV16J7du34+23387cAG2spaUFhYWF3d63Y8cOzJ49G4cffjiWL18eM68AcOutt+KZZ56BoiTeeZfoGkREZuGeYCLKekuXLkVLSwt++ctfxgVqAHDUUUfhBz/4AQDg888/x3HHHYeXXnop7rjjjjsODz74YPTr5uZm/PKXv8S4ceMwcuRIjBkzBtdccw0++ugjAMBVV12Ft99+G1988QWOO+44HHfccRg3blz08d9++y3uuOMOnH766TjxxBMxadIkvPzyyzHXjIznsccew9NPP43zzjsPo0aNwrXXXotdu3ZBSomHHnoIY8eOxUknnYQbb7wR+/fvjxv7O++8gyuuuAKjR4/GySefjBtuuAGffPJJzDHz5s3DySefjB07duD666/HySefjFtvvTXpvN5zzz3dzqvb7cbVV1+NsrIyTddoaWnBr3/9a5x99tkYOXIkxo8fj8ceewydP7BM5fV58MEHcdxxx+Gzzz7Dj370I5xyyimoqKjA3Xffjba2tpjHrlu3DpdffjlOPfVUnHzyyRg/fjwWLFjQ43MnIufjSjARZb233noLgwcPximnnGLoee+66y40NDTg+9//Po4++mjs378fH3zwAT777DOMGDEC06dPh8/nw+7du/Hf//3fAIBevXoBAFpbW3HVVVdhx44duPLKK3HkkUdizZo1mDdvHpqamqJBecRrr72G9vZ2XHXVVdi/fz+WLl2KW265BZWVldiwYQOuv/56bN++HU899RTmz5+Pe+65J/rYV155BfPmzcOZZ56JW2+9FX6/H88++yyuuOIKvPzyyzjyyCOjxwaDQUydOhXf+c53MHfu3IT7pN966y0cddRRGDVqVErz1t01pJS48cYbsWHDBlx00UUYPnw43n33Xdx777346quvcMcdd6R0jc5uueUWHHHEEZgzZw42btyI3/3ud2hqasK9994LAPjkk08wbdo0HHfccfjhD38Ij8eD7du3469//avuaxKR/TEIJqKs1tzcjK+++grnnXee4ed+5513cMkll2DevHnR266//vro/z/jjDPw5JNPoqmpCZMnT4557HPPPYfPPvsM//u//4tJkyYBAC677DJcddVV+M1vfoMLL7wQRUVF0eO/+uorvP766yguLgYAqKqKxYsXo7W1FS+++CLc7vCv83379uG1117Dz372M3g8Hhw8eBC//OUvcfHFF8fseZ4yZQqqqqqwePHimNsDgQCqqqowZ86chM+9ubkZe/bswfnnnx93X1NTE4LBYPTrwsLCmGC6u2v86U9/QmNjI2655RbceOONAMJbVX74wx/iySefxPe//30MGTIk4Zh6cuSRR+KRRx6JnrOoqAjPPPMMrr32Whx//PFYt24d2tvbsWTJEpSWluq6BhE5D7dDEFFWa25uBnBoBdZIJSUl+PDDD/HVV1+l/Ni1a9diwIABmDhxYvS2vLw8XHXVVWhpacFf/vKXmOOrqqqiATAAnHTSSQCASZMmRQPgyO3t7e3RMb333ntoamrChAkTsHfv3uh/iqJg1KhR2LBhQ9zYLr/88qTjj8xrd3t5r7rqKowZMyb639NPP530GmvXroXL5cJVV10Vc/u1114LKSXWrl2bdEw9ufLKK2O+/v73vx+9JhB+HQHgjTfegKqquq9DRM7ClWAiymqR1dSDBw8afu5bb70V8+bNwznnnIMRI0bg7LPPxne/+10MHjw46WO/+OILHHXUUXFJY0cffTQA4Msvv4y5vfO+WgDRgLin2w8cOIDBgwdj27ZtABC3vSKi82ozEN7HO2jQoKTjj/xR0dLSEnffz3/+cxw8eBDffPMNbrvttrj7u7vGF198gYEDB8aNJzIfX3zxRdIx9eSoo46K+XrIkCFQFAWff/45AKCmpgYvvPACfvzjH+P+++/HmDFj8J//+Z+oqqpKmtRHRM7FIJiIslpRUREGDhwYlwTWEyFEt7eHQqG422pqanDqqafij3/8I9atW4fHHnsMS5YswYMPPoizzz47rXF35XK5ur29pyAtkkwW+ffee+/FgAEDkp7X4/FoCvyKi4sxYMCAbuc1skc4EmR2pfUa3Unl9dF6joKCAjz99NPYsGED3n77bbz77ruor6/Hc889h8cff7zHuSciZ+OfuESU9c4991zs2LEDf/vb35Ie27t3bwDhfa2ddV2ZjRg4cCCuvPJKPPzww3jjjTfQp08fLFq0KHp/T0HbEUccge3bt8d9/L5lyxYAwOGHH550rFpEVqX79euH008/Pe6/iooK3ec+55xzsH37dmzatCntcR5xxBHYs2dPdJtFRGQ+InWGU319AGD79u1xX6uqGpMQqCgKxowZg//+7/9GfX09Zs2ahcbGxm63ixBRdmAQTERZ77rrrkNhYSF+/OMf45tvvom7f8eOHXjiiScAhFeO+/bti/fffz/mmGeeeSbm61AoBJ/PF3Nbv379MHDgQAQCgehtXq837jgAGDt2LL7++mvU19dHbwsGg/jd736HwsJC/Md//EfqT7QbZ511FoqKirB48WK0t7fH3b93717d577uuuvg9Xpxxx13dDuvqfRiGjt2LEKhUNz+4eXLl0MIgbFjxwLQ/vp01vWcTz31VPSaALotKTd8+HAAiHktiSi7cDsEEWW9IUOG4L777sOsWbNQU1MT7RgXCATwt7/9DWvWrMH3vve96PEXX3wxHn30UfzP//wPRo4ciffffx9bt26NOefBgwdx9tlnY/z48Tj++ONRWFiI9957D3//+99jqkWMGDEC9fX1uOeee3DiiSeisLAQ48aNw6WXXornnnsO8+bNw0cffYQjjjgCDQ0N+Otf/4o77rgjbm+sXkVFRfjpT3+K22+/Hd/73vdQU1OD0tJSfPnll3jnnXdwyimn4Cc/+Ymucw8dOhT33Xcf5syZg6qqqmjHOCklPv/8c6xcuRKKomjaYzxu3DhUVFRg4cKF0brK69atwxtvvIEf/OAHMZUhtLw+nX3++eeYPn06zjrrLGzcuBErVqzAxIkTo+2cH3roIbz//vs4++yzccQRR+Dbb7/FM888g0GDBuE73/mOrrkhIvtjEExEOeG8887DihUr8Nhjj+GNN97As88+C4/Hg+OOOw7z5s3DJZdcEj325ptvxt69e9HQ0IDVq1dj7NixWLp0KcaMGRM9pqCgAJdffjnWrVuH119/HVJKDBkyBHfddReuuOKK6HFXXHEFPv74Y7z00ktYvnw5jjjiCIwbNw4FBQX43e9+h/vuuw8vv/wympubUV5ejnvuuScmIDfCf/3Xf2HgwIF49NFH8dhjjyEQCOCwww7Dqaeemva1zj//fLz22mt4/PHHsW7dOrz44osQQuDwww/H2WefjcsvvzwabCaiKAoeeeQRPPDAA6ivr8dLL72EI444ArfffjuuvfbamGO1vD6d/eY3v8Fvf/tb3H///XC73fj+97+P22+/PXr/uHHj8MUXX+DFF1/Evn370LdvX5x22mmYOXNmTEUOIsouQqbyeRUREZFDPPjgg6irq8P69etZ/5eI4nBPMBERERHlHAbBRERERJRzGAQTERERUc7hnmAiIiIiyjlcCSYiIiKinMMgmIiIiIhyDoNgIiIiIso5bJaRIiklVDU7tlErisia5+IEnG/rcc6txfm2HufcWpxva+mdb0UREEIkPY5BcIpUVWLv3oOZHkba3G4Fffv2QlNTC4JBNdPDyXqcb+txzq3F+bYe59xanG9rpTPfpaW94HIlD4K5HYKIiIiIcg6DYCIiIiLKOQyCiYiIiCjnMAgmIiIiopzDIJiIiIiIcg6DYCIiIiLKOQyCiYiIiCjnMAgmIiIiopzDIJiIiIiIcg6DYCIiIiLKOQyCiYiIiCjnMAgmIiIiopzDIJiIiIiIcg6DYCIiIiLKOQyCiYiIiCjn2CoI3r59O37yk59g8uTJOOGEEzBx4sRuj3vhhRcwfvx4nHjiiZg0aRLeeuutuGN8Ph/uuOMOnHbaaTj55JPxwx/+EHv27DH7KRARERGRA9gqCP7kk0/wzjvv4KijjsLRRx/d7TGrVq3CnXfeierqaixZsgSjR4/GjBkzsHHjxpjjbrnlFqxbtw4//elPcd9992Hr1q24/vrrEQwGLXgmRERERGRn7kwPoLNx48bh/PPPBwDMmzcPmzdvjjvmgQcewIQJE3DLLbcAACorK/Hvf/8bDz30EJYsWQIA+Nvf/oY///nPeOyxx3DmmWcCAMrLy1FTU4PXX38dNTU11jwhIiIiIrIlW60EK0ri4ezcuRPbtm1DdXV1zO01NTVYv349AoEAAGDt2rUoKSnBGWecET1m2LBhGD58ONauXWv8wImIiIjIUWwVBCezZcsWAOFV3c6OPvpotLe3Y+fOndHjysvLIYSIOW7YsGHRcxARERFR7rLVdohkDhw4AAAoKSmJuT3ydeT+pqYmFBcXxz2+d+/e3W6xSJXb7ai/HeD3Az4fUFwMeL3h21wuJeZfMhfn23qcc2txvq3HObcW59taVsy3o4JgO1AUgb59e2V6GJr8+c/AggXAq68CqgooCjB5MjBnDhDZKVJS4s3sIHMM59t6nHNrcb6txzm3FufbWmbOt6OC4N69ewMIlz8bMGBA9PampqaY+0tKSrB79+64xx84cCB6jF6qKtHU1JLWOazw+ONu3HabBy4XoKrhbSGqCrz2msQrrwD339+OWbM8aGryIxRSMzvYHOByKSgp8XK+LcQ5txbn23qcc2txvq2VznyXlHg1rSA7KggeNmwYgPCe38j/j3ydl5eHwYMHR49bv349pJQx+4K3bt2KY489Nu1xBIP2/uZvbHThtts8kFKga0W4YDA8H3Pm5OG004ATTlBt/3yySSjE+bYa59xanG/rcc6txfm2lpnz7aiNLYMHD8bQoUOxZs2amNvr6+sxZswYeDweAMDYsWNx4MABrF+/PnrM1q1b8Y9//ANjx461dMyZsGhRHpIU2oDLBSxcaM14iIiIiOzGVivBfr8f77zzDgDgiy++QHNzczTgPe2001BaWoqZM2fi1ltvxZAhQ1BRUYH6+nps2rQJTz31VPQ8J598Ms4880zccccdmDt3LvLz87Fw4UIcd9xxuOCCCzLy3Kzi9wNr1rijWyB6EgwKvPwy8MADQF6eRYMjIiIisgkhpZSZHkTE559/jvPOO6/b+5588klUVFQACLdNXrJkCb788kuUl5dj9uzZOPfcc2OO9/l8uOeee/DHP/4RwWAQZ555Jn784x/jsMMOS2uMoZCKvXsPpnUOM+3ZIzByZJHm4//5z4MoLdX+MUO40oRAcbGMVpqg5NxuBX379sK+fQf5MZpFOOfW4nxbj3NuLc63tdKZ79LSXpr2BNsqCHYCuwfBfj9QXl6UdCUYCFeL2LnzIPLykn9zNTa6sGhRXnSVWVEkqqqCuPHGdlRUhIwYelbjL0/rcc6txfm2HufcWpxva1kRBDtqTzAl5/UCVVVBuFyJ/7ZxuyWmTIGm1dxly/IwebIXDQ3uTpUmBBoa3Jg0yYvly7mfgoiIiJyFQXAWmj69HWqSP5pCIWDWrOTnamx0Yd68fEgpEArFri6HQgJSCsydm48NG1xpjJiIiIjIWgyCs1BlZQjz57dBCBm3IuxySQghcd99gWjDjES0VJpQFGDxYq4GExERkXMwCM5StbXtWLHCj+rqIBQlHAgrikR1dRArVvhxzTXBJGc4VGmi6wpwV6GQQH29G36/IUMnIiIiMp2tSqSRsSoqQqioCPVQ0SH53z8+n9CUYAeE9wj7fAJeL/MsiZzNDyF8kLIYAEvA2AdfFyKjMQjOAV4vdAWnxcUSiiI1VpqQKC5mAEzkVG73ehQW1sHjWQUhVEipIBCYgJaWmQgGKzM9vJyV6HUBTs/08IgcjdshqEdaK024XBI1NUHWDSZyqIKCpejTpwoez2oIEc6qFUKFx7MaffqMR0HBYxkeYW5K9rp4PEszPEIiZ2MQTAlpqTShqsC0ae3WDIiIDOV2r0dR0RwIISFEbK6AEEEIIVFUNBtud2OGRpibtLwuhYWzAKzLzACJsgCDYEpIS6WJ+fPb2DCDyKEKC+sAJCtx6Oo4jqyi9XUBFlowGqLsxCCYkkpWaaK2lqvARM7k79hrmrhajBBBeDwrAbAEjDW0vy7Ay+DrQqQPE+NIk8SVJojIiYTwRfeaJj9W7ahOwB98s6XyugAqhGgCMMDMIRFlJQbBlBK9lSaIyH6kLIaUiqaAS0qlozwXmS2V1wVQIGWJ6WMiykbcDkFElLO8CAQmQMrE6yFSuhEITATr01pF++sCTAFfFyJ9GAQTEeWwlpYZAJIltoY6jiOraH1dgFkWjIYoOzEIJiLKYcHgGDQ3L4CUIm7lUUo3pBRobl7AhhkW0/K6tLQsBHBGZgZIlAUYBBMR2Z4fQuyBWVUAWlunYv/+BgQCNZAy/LYQ7kxWg/37G9DaOtWU66bH3Dmxg2SvSyBwXYZHSORsTIwjIrIpK1sZB4OVaGqqRDi49HUkwdlvr2mutXdO9Lq4+Q5OlBauBBMR2VDmWhl7IeVA2DEAzu32zvZ9XYicikEwEZHNsJVxPM4JERmNQTARkc2wlXE8zgkRGY1BMBGRrbCVcTzOCREZj0FwFvL7gT17BPx8HyByHD2tjLMd54SIzMAgOIs0NrpQW1uA8vIijBxZhPLyItTWFmDDhmQfIRKRXURa5mo7NjdaGXNOiMgMDIKzxLJleZg82YuGBjdUVQAAVFWgocGNSZO8WL48L8MjJCJt2Mo4HueEiIzHIDgLNDa6MG9ePqQUCIVEzH2hkICUAnPn5nNFmMgh2Mo4HueEiIzGIDgLLFqUByXJK6kowOLFXA0mcgK2Mo7HOSEiozEIdji/H1izxh23AtxVKCRQX+9mshyRQzizlbG5OCdEZCQ2XXQ4n09E9wAno6oCPp+A1ytNHhURGcH4VsZWt0Q2/npOae9MRPbHINjhioslFEVqCoQVRaK4mAEwkfN4IaX+QM/tXo/CwrqOWrtqx+rpBLS0zDRl+4A110tvToiIuB3C4bxeoKoqCJcrcXDrcknU1ATh5XsGUU4pKFiKPn2q4PGsjtbaFUKFx7MaffqMR0HBY4Zez+NZYun1iIj0YhCcBaZPb4eapI68qgLTprVbMyAisgW3ez2KiuZACBnXbU2IIISQKCqaDbe70aAr/hmFhbMtvB4RkX4MgrNAZWUI8+e3QQgZtyLsckkIITF/fhsqKpKVFyKibFJYWAcgWWlEV8dxRlhg8fWIiPRjEJwlamvbsWKFH9XVQShKOBBWFInq6iBWrPCjtja3V4HNbCXNNtVkT/6OPbnBhEcJEYTHsxJAut/AfgCvWni9+OsLsceE8xJRtmJiXBapqAihoiIEvz9cNaK4WOb8HuDGRhcWLcrDmjXhTnqKIlFVFcSNN7anvTJu5rmJ0iWEL7onN/mxakelBf2/MIRoAmDd9SKsTvojouzBleAs5PUCAwcyADazlTTbVJPdSVkcraWb/Filo9RYOtcrgda3FCOuB1if9EdE2YVBMGUlM1tJs001OYMXgcCEuO5qXUnpRiAwEenX2vUCmGzZ9axP+iOibMMgmLKSma2k2aaanKKlZQaAZFtzQh3HGWG2ZdezPumPiLINg2DKOma2kmabanKSYHAMmpsXQEoRt0IrpRtSCjQ3LzBw7+yZaGlZaMH1rE76I6JsxCCYso6eVtJ2ODflIvMrGrS2TsX+/Q0IBGqie4TDyWM12L+/Aa2tUw29XiBwnenX05P0R9mAFUDIWKwOQVnHzFbSbFNNRrC6okEwWImmpkqEgwhfR1KaeZmzZl8vkvSnJRA2KgmPMocVQMgsXAmmrGNmK2m2qaZ0ZbaigRdSDoSZAbA117M66Y8yhRVAyEwMgikrmdlKmm2qSS9WNDCO9Ul/ZDX+vJDZGARTVjKzlTTbVJNerGhgHOuT/shq/HkhszEIpqxlZitptqmm1LGigdGsTvojK/HnhczHxDjKama2kmabakqF1W2MjWVNQp0eVif9HWLfOckGzv55IadgEEw5wesFvF5zKjWYeW7KHk6saOCsrHyvJUGQs+bEuZz480LOw+0QRESWcFZFA2blx+OcWMlZPy/kTAyCiYgs4pSKBszKj8c5sZ5Tfl7IuRgEExFZxCkVDZiVH49zYj2n/LyQczEIJiKykP0rGjArPx7nJFPs//NCTsbEOCIii2WuokFyzMqPxznJLDv/vJCzMQgmIsoYayoapIJZ+fE4J3Zhv58XcjZuhyAiok68aG+vhExS9U9KoL19DHJjRY6VCoiyEYNgIiLqQhh8nPOxUgFR9mEQTEREnfiRl7ceIkl8KwSQl/ceciUJjJUKiLIPg2AioqzghxB7kG5QqicJLFdYV6nAmNeSiBJjYhwRkYMZ3caXSWCJmVmpgC2ZiazFlWAiIocyp41vJAks8dtDOEDL5SQwL6QcCKOeP1syE1mPQTARkQOZ2ca3re0cAMlWglW0tZ2b8rkpHlsyE2UGg2AiIgcys41vfv7bSP72oCA//62Uz03x2JKZKDMYBBMROY6ZbXz90T2pic+tskWwIdiSmShTGASTbfn9wJ49An7+zieKYWwFh9hKBKwOYS3ON1HmMAgm22lsdKG2tgDl5UUYObII5eVFqK0twIYNyT4uJMoNkQoO2o7tvoKD270eJSVXon//MvTvfwz69y9DScmVcLk+TvvcpJ0RryUR6cMgmGxl2bI8TJ7sRUODG6oartavqgINDW5MmuTF8uV5GR4hkR2k18Y3cSWCSQgGR7BFsGXYkpkoUxgEk200Nrowb14+pBQIhWLbVYVCAlIKzJ2bzxVhIuhv46ulEoHb/XcAifeoskWwcdiSmSgzGASTbSxalAclyXekogCLF3M1mEhvG19tlQjcCAZPYotgi7AlM1FmMAgmW/D7gTVr3HErwF2FQgL19W4myxFBTxtf7ZUI3O7N2L9/hQUtggmwsiUzEUWwbTLZgs8nonuAk1FVAZ9PwOuVJo+KyP5SaeObaiWCUGg4mpqe0nRue3LKuMPjDAZHOXy+iZyFQTDZQnGxhKJITYGwokgUFzMAJorlhZSJA6ZIJQItgXBsJYLk57YTt3s9CgvrovWOwyuqE9DSMtNWWwqcMk6ibMXtEGQLXi9QVRWEy5U4uHW5JGpqgvA65/2YyEayvxJB4soX41FQ8FiGRxjmlHESZTMGwWQb06e3Q02yQKWqwLRp7dYMiCgLZXMlAi2VL4qKZsPtbszQCMOcMk6ibMcgmGyjsjKE+fPbIISMWxF2uSSEkJg/vw0VFcnewImoJ9lciUBb5QtXx3GZ45RxEmU7BsFkK7W17Vixwo/q6iAUJRwIK4pEdXUQK1b4UVvLVWCidGVnJQLtlS88npWItIm2XqbHGdsmmyiXMTGObKeiIoSKihD8/nDViOJiyT3ARAZLpaqEE6Ra+SL8nK1/vpkaJ5PwiOJxJZhsy+sFBg5kAExkLi+kHAgnB8DAocoX2o7tXPnCWpkYJ5PwiLrHIJiIiLKAUypfWDtOJuER9YxBMBERZQWnVL6wcpxMwiPqGYNgIiLKCk6pfGHdODOdhEdkbwyCiYgoazil8oUV49SThEeUS1gdgoiIsopTKl+YPU79bbKJcgNXgomIKEs5pfKFWeN0SrIgUWYwCCYiIspSTkkWJMoEBsFERERZyinJgkSZwCCYiIgoizklWZDIakyMIyKiFNk74YziOSVZkLKRfb/nGAQTEZEmbvd6FBbWddSeVTtWEyegpWUmgNMzPTzSxAsp7RWIUHZK9PvCLttvuB2CiIiSKihYij59quDxrI6W3BJChcezGn36jIfHszTDIyQiu0j2+6Kg4LEMjzCMQTARESXkdq9HUdEcCCHjuo8JEYQQEoWFswCss3RcEsC3QmCHIvCtEJCWXp2IuqPl90VR0Wy43Y0ZGuEhDIKJiCihwsI6AK4kR7kALLRgNMABATzqzUNFaS8M71+EU/sVYXj/IlSU9sKj3jwcEJYMg4i6ofX3Rfi4zGIQTLbh9wN79gj42b6eyEb8HXv6ggmPCt//MgBzf4DfzHNhVL8i3NkrH9uV2Gh3uyJwZ698jOpXhDfzkr0JE5HxtP++8HhWwuzfF8kwCKaMa2x0oba2AOXlRRg5sgjl5UWorS3Ahg18EyPKNCF8mtruhqkQosm0sbyZ58KVvb1oBSCFgBSxQXDktlYAV/b2MhAmslgqvy+EUCGEz+QRJcYgmDJq2bI8TJ7sRUODG6oafkNTVYGGBjcmTfJi+fK8DI+QKLdJWRytLZucAilLTBnHAQFc29sLCUAVifc7qB37g6/t7eXWCCILpfL7Qkqlo2xa5jAIpoxpbHRh3rx8SCkQCsW+U4VCAlIKzJ2bzxVhoozyIhCYENdtrKvw/VNgVh3Q5wry4EfyADhCFQJ+AM8X8A9pIuto/30RCExEpusGMwimjFm0KA9Kku9ARQEWL+abGFEmtbTMABBKclQIwCxTri8BLPV6dD12idfDqhFEFtL6+yJ8XGYxCKaM8PuBNWvccSvAXYVCAvX1bibLEWVQMDgGzc0LIKWIW+GR0g0pBVpaFgI4w5Tr7xUC21xK3B7gZGTH4/ZxSwSRZbT8vmhuXmCLhhkMgm0sm6sl+Hwiugc4GVUV8PnSexfL5rkkskJr61Ts39+AQKAmuucv3AGqpuP260y79sE0g9jmFINnIkpPst8Xra1TMzzCMLZNtqHGRhcWLcrDmjXhZDFFkaiqCuLGG9tRUZHsIwZnKC6WUBSpKRBWFIniYn0faObCXBJZJRisRFNTJQA/hPB1JLWE9/S5TXw36ZXmfoYiyQ0RRFZL9PvCLrgSbDO5Ui3B6wWqqoJwuRK/OblcEjU1QXh1/NzkylwSWc8LKQfCqje0UikxNKRCpBjMio7H9WUMTJRB1v6+SAWDYBvJtWoJ06e3Q01STlBVgWnT2lM+d67NJVE2EwCu8wd0PfZ6fwDcDEFE3WEQbCO5Vi2hsjKE+fPbIISMWxF2uSSEkJg/v03XtoVcm0uibHdpazu8ABSNq8GKlPACuKQ19T+iiSg3MAi2iVytllBb244VK/yorg5CUcJvbooiUV0dxIoVftTWpv4GlqtzSZTNekvg8QN+CCQPhBUpIQAsO+BHb26FIKIeODIx7o033sCiRYvw6aefolevXvjOd76DW2+9FYMHD4457oUXXsDSpUvx5Zdfory8HLNmzcK5556boVEnpqdagtebHb/dKypCqKgIwe8Pz0NxsdS1Bzgil+eSKJuNaw/h6QN+XNvbC39HINy5bFpkz3ABwgHwue1MfiWinjluJXjDhg2YMWMGjjnmGDz00EO444478M9//hPXXnstWltbo8etWrUKd955J6qrq7FkyRKMHj0aM2bMwMaNGzM3+AQi1RK0SKdagpUkgG+FwA5F4NuONqaJeL3AwIHpBcBAds4lEYWNaw/hw2+bcffBNhylxv7sHqVK3H2wDZu+bWYATERJOW4leNWqVTj88MPxq1/9CqJjBaC0tBQ/+MEPsHnzZpx66qkAgAceeAATJkzALbfcAgCorKzEv//9bzz00ENYsmRJpobfo0i1hIaGxB/ju1zhrQLpBopmOiDCLU6Xej3Y5jr0d9bQkIrr/AFc2tpu6keU2TSXRBSvtwSu97fjOn879olwHeAiKdFXgklwRKSZ41aCg8EgevXqFQ2AAaC4uBgAIDs+Ctu5cye2bduG6urqmMfW1NRg/fr1CAT0ZRmbzcxqCVZ5M8+FUf2KcGevfGxXYt+OtisCd/bKx6h+RXgzz9yqDNkwl0SUmABQKoEhqkQpA2AiSpHjguDvfe97+Oyzz/D000/D5/Nh586dWLBgAU444QSccsopAIAtW7YAAMrLy2Mee/TRR6O9vR07d+60fNxamFktwQpv5rlwZW8vWhHep9e1xWnktlYAV/b2mhoIO30uiYiIyFyO2w5x6qmnoq6uDnPmzMHPf/5zAMDw4cOxdOlSuFzhoOrAgQMAgJKSkpjHRr6O3K+X223e3w7XXRfCyJGteOSRPKxa5Yp2OaupCeHGG9tRWanCiL9dXB3bFFwuY57LAQFM7e2FBKAmaVGqCgFFSkzt7cXmAy2mbY2wai61MHq+KTnOubU439bjnFuL820tK+bbcUHwX//6V9x+++245JJLcM4552D//v14+OGHccMNN+CZZ55BQUGBqddXFIG+fXuZeo3q6vB/fj/Q1ASUlAh4vW4Y+XL5/cBXXwElJV50+VtBlycBtABJk98iVCHQAmBFn174YfqX75EVc5mKkhJuQLYa59xanG/rcc6t4gfwVceCmrlxAB1i5ve344Lgu+++G5WVlZg3b170ttGjR+Occ87Bq6++iksvvRS9e/cGAPh8PgwYMCB6XFNTEwBE79dDVSWamlp0Pz5VHg/Q2hr+zwiNjQoefjgP9fWxK6M33RRZGU2dBPDbEi+gCCDJKnDsAyV+o0p8v8lvyV4+o+cyFS6XgpISL5qa/AiF9M0zpYZzbi3Ot/U459Zwud5DQUEd8vJWQggVUipob5+I1taZCIXGZHp4WSud7++SEq+mFWTHBcGfffYZzjvvvJjbBg0ahL59+2LHjh0AgGHDhgEI7w2O/P/I13l5eXH1hFMVDDrzl82yZXmYNy8fioJoHV1VFVi92oVVq1yYP79NV3OKb4XAVh0fV0ghsNUl8HVIRWmOVCkLhVTHfv84FefcWpxv63HOzVNQsBRFRXMAuCBEeI6FUJGXV4+8vNfQ3LwAra1TMzvILGfm97fjNrYcfvjh+Mc//hFz2xdffIF9+/bhiCOOAAAMHjwYQ4cOxZo1a2KOq6+vx5gxY+DxeCwbr100Nrowb14+pBRxZcNCIQEpBebOzceGDaknqx1Mcxm3OZXVYyIiIgu43etRVDQHQkgIEYy5T4gghJAoKpoNt7sxQyOkdDluJfiyyy7Dr371K9x9990YN24c9u/fj0ceeQT9+vWLKYk2c+ZM3HrrrRgyZAgqKipQX1+PTZs24amnnsrg6DNn0aI8KAoQSlAMQVGAxYvzUq6Y0CvNVdyiJC1QiYiIrFZYWAfABSCY4CgXCgvr0NRUadGoyEiOC4KvvvpqeDwePPvss3jxxRfRq1cvjB49Gr/5zW/Qt2/f6HETJ06E3+/HkiVL8Oijj6K8vBx1dXU4+eSTMzj6zPD7gTVr3ElbCYdCAvX1bvj9SKmBRKmUGBpSsV2JL4uWiJASR6nhAvdEZGd+COGDlMUAmIRFucAPj2dVdAtET4QIwuNZiXDSHH82nMZxQbAQApdffjkuv/zypMdefPHFuPjiiy0Ylb35fCJpAByhqgI+n4DXqz0yFQCu8wdwZ6/8lMd2vT/AAvdENuV2r0dhYV00GJBSQSAwAS0tMxEMcuWLspcQvqQB8KFj1Y4/EhkEO43j9gRT6oqLJRRFW1CrKBLFxakvzV7a2g4vAEXj1gZFSngBXNLKjm1EdlRQsBR9+lTB41kdkxDk8axGnz7jUVDwWIZHSGQeKYshpbYQSUql41MSchoGwTnA6wWqqoJxndO6crkkamqCKW2FiOgtgccPhEudJQuEFSkhACw74DetUQYR6ceEICIvAoEJkDLxB+ZSuhEITAS3QjgTg+AcMX16O9Qkn+yoKjBtmv6V2XHtITx9wI8ChPf7ii7BcOS2AgDPHPDj3Ha2LCayo0MJQYm4Oo4jyk4tLTMAJHufCnUcR07EIDhHVFaGMH9+G4SQcSvCLpeEEBLz57elXBmiq3HtIXz4bTPuPtiGo9TY6xylStx9sA2bvm1mAExkW5GEoEQZ8V0TgoiyTzA4Bs3NCyCliFsRltINKQWamxdwf7yDOS4xLpf5/eEkt+JiqWvLQm1tO4YPV7F4cR7q693RjnHV1UFMm9aedgAc0VsC1/vbcZ2/HftEuA5wkQxXgWASHFEyma3EwIQgokNaW6ciGBzRkSC6slOCaA1aWmYwAHY4BsEO0NjowqJFedEyZ4oiUVUVxI03ph64Shne9hDZqRD52gwCQKkMl1AjosTsUokhkhCkJRBmQhDlgmCwEk1NlXC729C3bwj797sQDKZeDYnsh9shbG7ZsjxMnuxFQ4M7ptVxQ4MbkyZ5sXx5nq5zSRk+l5T6zkVExrFXJQYmBBF1zwvgMPB7PnswCLYxI1sdm9k2mYj0s2MlBiYEEVEuYBBsY5FWx4lEWh1beS4iMo4dKzEwIYiIcgGDYJuKtDruumrbVedWx1aci4iMZN9KDK2tU7F/fwMCgZpo04BIQtD+/Q1obZ1q2ViIiMzAxDibSrXV8ddfCwwZ0n0Cmtltk4lIH7tXYogkBAF7oSi7oaqDAJRadn0iIjNxJdimUml1DACnndYLtbUF3e7ptaJtMhGlzu6tWd3u9SgpuRL9+w9Dv36V6N9/GEpKrmSnOCLKCgyCbUprq+OIRBUjrGibTER62LcSg70qVhARGY9BsI1paXXcWaIqD1a0TSai1NmxEoMdK1YQERmNQbCNJWp1nEh3VR6saptMRKmxYyUGO1asICIyGoNgm6utbceKFX5UV4dXX7ToqcpD53NF9ghH2iavWOFHbS1XgZPx+4E9ewQraJChkldimAJF+QeAvRaMxo4VK/wQYk+Sa2k5hojoEFaHcICKihAqKkLYsUPg1FOLND2mpyoPkXO1tytwuXohFGpBXp5JfZOziJGtq4m6c6gSg7+jCkQx8vOfQknJ1VCU3RAi0uZ8EA4enIu2NnNKlNmpYoWWVtJ2aTdNRM7DlWAHGTDAuCoPXi9w2GFgEpwGRrauJkrOCykHorj4JhQXz4kGwAAgBKAou1FcPAvFxdeacnW7VKzQkpjH5D0iSgeDYAdhlQfrsd00ZUJ+/hLk578IIRANgCMit+Xn/wH5+WYEeZmvWKEtMW8WiopmM3mPiHRjEOwwrPJgLbabpkzo1et/NR53rynXz3TFCm2JeQCQrAkQk/eIqGcMgh2GVR6sw3bTlBl7Y7ZA9CS8NWIXzEiWy2zFCq2JeUiaLJyJdtNE5BwMgh2IVR6soafdNFG6tATAEZE9wsYKV1lobb0iScWKzCfmaTtfOHmPyHlY8cRsrA7hUJEqD35/OFgrLpbcA2ywSLtpLYEw202TUVR1EKSM3wvcnUi1CCMkqrLQ1LQkWrHC7K51kcQ8owLhTLSbJkoHK55YhyvBDuf1AgMHMgA2AxMRKTNKo4FwIuEAuAxAadpXTF5l4RlIORDWtG3WmpgHSJn4L4VMtJsmSgcrnliLQTBRAkxEpEw4ePA2jcfdnva17NgiWVtiHgAk+/TF2nbTROmw489itmMQTJQAExEpE9rarkdb20Udq52x90Vua2u7yJCGGXZskawtMW8hmpsX2qrdNFE67PizmO0YBBMlwUREygSf73H4fAuhqmXRQDiyBcLnWwif73EDrmLHFslhyVtJT9V0DJEz2PdnMZsxMY7S0l1inpnJekaeO5VzMRGRMqGtbWrHam+4bFo4Ca6nPcD+lJPXUm+R/DWkHKLpeCN010q663PTcgyR3dmpXXkuYRBMujQ2urBoUR7WrAm3ElYUidNOC0EIYMMGV/S2qqogbryxPe3tAt1dT++50zmX1wt4vawCQVYrhap2H/ymk0meaiWGfv1OylCWulfDG76WY4jsKZWfRVY8MQ63Q1DKli3Lw+TJXjQ0uKPlw1RVoLHRhfXrXTG3NTS4MWmSF8uX6++o1tP19JzbyHMRZVr6meTaKjFEMEudyCyZb1eeixgEU0oaG12YNy8fUopuOqkJdG1jGgoJSCkwd24+NmzQ0gZV+/VSPbeR5yLKNKMyybVXYkj93ESkXabbleciBsGUkkWL8qDo+K5RFGDx4tRXWbVcT+u5jTwXUaYZlUmeqBJDuucmIu0y2648NzEIJs38fmDNGnc3K8DJhUIC9fVu+FNIaNV6PS3nNvJcRJlnbCZ5bJUFbT/fzFInMh4rnliLiXE5wKiKBj6f0NRCuCeqKuDzCc2JZalcL9m5jTwXUaaZkUkeqbIgxHb073+ioecmIu1Y8cQ6XAnOYo2NLtTWFqC8vAgjRxahvLwItbUFuve8FhfLaJ1cPRRForhY++NTuV6ycxt5LqJMi2SSazsWKC7+oeb9u1IOTOHczFInMo/XwnbluYlBcJZKVgVh2bLUPwTweoGqqmBc5zQtXC6JmppgSivRWq+n5dxGnoso87RXdRAC8HheT6GiA7PUiSg3MAjOQlqqINx6qwfr1qV+7unT26Fq+xQ2hqoC06al3llNy/W0ntvIcxFlWipVHVKt6MAsdSLKBQyCs5CWKgguF7BwYernrqwMYf78Ngghu1lVlR3/db6OhBAS8+e36WqYkeh6qZ7byHMRZZq+qg7aKjowS52IcgGD4CyjtQpCMCjw8svQVQWhtrYdK1b4UV0djO6zVRSJ008P4fTTQzG3VVcHsWKFH7W1+ldXe7qennMbeS6izPFDiD1obb2iI5N8PKSGXUqpVHRgljoRZTtWh8gyqVVBAHw+oLT7bqwJVVSEUFER6rbyhFHVKLReL5PnIrJSTy2S/f6rkZ+/StM5UqnowCx1IspmDIKzTKQKgpZAWFGA4jQTu71exJUS6+42oxh5bjPHSWS0goKlKCqaA8AV1yLZ43kNUgoIkfz7WV9FBy/LoBFR1uF2iCyjtQqC2y0xZQq4AkrkAMlbJAOAhJSJyx+yogMR0SEMgrOQlioIoRAwa5Y14yGi9GhtkcyKDkRE2jEIzkJaqiDcd18AZ5yRoQESUQq0tkgOARCs6EBEpBGD4CyVrArCNdckfkMlIntIrUWyxIEDv2dFByIiDZgYl8USV0Hg3z9EThBpkawlEJZSQXv7OWhvrwYrOhARJcZIKAd4vcDAgSwDRuRMetsYeyHlQDAAJiLqHoNgIiKbYxtjIiLjMQgmIrI5tjEmIjIeg2AiIgdgG2MiImMxMY6IyCHYxjhVnKd4nBOiCAbBRESOwzbGibjd61FYWNdRX1ntWDGfgJaWmTm7ZYRzQhSP2yGIiChrFBQsRZ8+VfB4VkfLygmhwuNZjT59xqOg4LEMj9B6nBOi7jEIJiKirOB2r0dR0RwIIeM67AkRhBASRUWz4XY3ZmiE1uOcEPWMQTAREWWFwsI6AK4kR7k6jssNnBOinjEIJiKiLODv2O+auCW8EEF4PCsB+K0ZlqX8EGIPDj03zglRIkyMIyIixxPCp6m1dPhYtaNCQnYkF7pc76Gw8MG4pDe///s5OydEWjAIJiIix5OyGFIqmoI+KZWOEmHZ4BEUF98MwBWX9ObxvAYpBYSQSc+SXXNCpA23QxARURbwIhCYENdRrysp3QgEJiIbauS6XO8BuDlB0hsASEiZeE9wNs0JUSoYBBMRUVZoaZkBIJTkqFDHcc5XUKAt6S2X5oQoFQyCiYgoKwSDY9DcvABSirgVYSndkFKguXlBljSH8CMvbyWAZElvIQAiR+aEKDUMgomIKGu0tk7F/v0NCARqIGX4LS6cKFaD/fsb0No6NcMjNEZqiYASBw78PuvnhChVTIwjIqKsEgxWoqmpEuGSYb6OhK/s2u+aaiJge/s5aG+vRjbPCVGquBJMRERZygspByI7gz0v2tsnItlaVnzSWzbPCVFqGAQTERE5UGtrbiUCEhmNQTAREZEDhUKnA3iYSW9EOjEIJiIicqzp8PleZ9IbkQ5MjKMovx/w+QSKiyW83C5GROQIodAYNDVVgElvRKnhSjChsdGF2toClJcXYeTIIpSXF6G2tgAbNiQrwk5ERPbBpDeiVDAIznGPP+7G5MleNDS4oaoCAKCqAg0Nbkya5MXy5XkZHiERERGR8RgE57A//xm47TYPpBQIhUTMfaFQuMPQ3Ln5XBEmIiKirMMgOIctWAC4ksS3igIsXszVYCIiIsouDIJzlN8PvPoqEAyKhMeFQgL19W74/RYNjIiIiMgCDIJzlM8HqNrazkNVBXy+xMEyxfL7gT17BP94ICIisikGwTmquDi81UELRZEoLpbmDihL9FRpo7GRP2pERER2wnfmHOX1ApMnA2534uDW5ZKoqQmybrAGy5bl9VhpY8KEAixalOEBEhERURSD4Bw2ezYQStJ2XlWBadParRmQgzU2ujBvXn7CShs33QSuCBMREdkE35Fz2JlnAvfdF4AQEi5X7IqwyyUhhMT8+W2oqEgSKRMWLcpLur3E5QIeeYSVNoiIiOyAQXCOu+aaIFas8KO6OghFCQfCiiJRXR2+vbaWq8DJ+P3AmjXuuBXgroJBYNUqF5PliIiIbMCd6QFQ5lVUhFBREYLfD/h8AsXFknuAU+Dziege4GQilTa8XiYaEhERZRKDYIryesHgTIfiYglFkZoCYVbaICIisgduhyBKk9cLVFUF4/ZVd+V2AxMmhLjKTkREZAMMgokMMH16e9LmI6EQcOON3GNNRERkBwyCiQxQWRnC/PltCSttPPwwUFmpsU0fERERmYpBMJFBamvbe6y0sWpVK6ZPz/AAibKaH0LsAcDyK0SkDRPjiAzUU6UNt5t/bxKZwe1ej8LCOng8qyCECikVBAIT0NIyE8FgZaaHR0Q2xndmIhN4vcDAgSw1R2SmgoKl6NOnCh7PaggR3mokhAqPZzX69BmPgoLHMjxCIrIzBsFEROQ4bvd6FBXNgRASQgRj7hMiCCEkiopmw+1uzNAIicjuGAQTEZHjFBbWAXAlOcrVcRwRUTwGwURE5DD+jj3AwYRHCRGEx7MSTJYjou4wCCYiIkcRwhfdA5z8WBVC+EweERE5EYNgIiJyFCmLIaW2ty8pFUhZbPKIiMiJGAQTEZHDeBEITICUiat8SulGIDARAMu0EFE8BsFEROQ4LS0zAISSHBXqOI6IKB6DYCIicpxgcAyamxdAShG3IiylG1IKNDcvYMMMIuoRg2CyDb8f2LNHwJ8Nidx+P8SePciOJ0NkT62tU7F/fwMCgZroHuFwx7ga7N/fgNbWqRkeIRHZGdsmU8Y1NrqwaFEe1qxxQ1UFFEWiqiqIG29sR0VFso877cXduB6Fi+rgWbMKQlUhFQWBqglom/FDoPr8TA+PKOsEg5VoaqoE4IcQvo4kOO4BJqLkuBJMGbVsWR4mT/aioSEcAAOAqgo0NLgxaZIXy5fnZXiE2hUsW4o+k6vgaVgNoXa0cFVVeBpWo3jCBcCiRRkeIVE280LKgWAATERaMQimjGlsdGHevHxIKRAKiZj7QiEBKQXmzs3Hhg3JukJlnrtxPYrmzYGQEiLUpYVrKAghJXDTTXA1rs/QCImIiKgzBsGUMYsW5UFJ8h2oKMDixfZfDS5cVAcoSYJ1lwsFj7CFKxERkR0wCKaM8PuBNWvccSvAXYVCAvX1bnvnl/n94T3AocQtXBEMIm/Va0yWIyIisgHdQXAoFMKqVavwk5/8BDfffDP+9a9/AQB8Ph9ef/11fPPNN4YNsjsvv/wyvvvd7+LEE09ERUUFrrvuOrS2tkbvf/PNNzFp0iSceOKJGD9+PF588UVTx0Op8flEdA9wMqoq4PNpOzYThM8X3QOc9FhVhfAlaOHataoEq0zE45wQEZEBdFWHaGpqwnXXXYdNmzahsLAQfr8f3//+9wEAhYWFuPvuu/Hd734Xs2fPNnSwEY888giWLFmC6dOnY/To0di3bx/Wr1+PUChcSeD999/HjBkzcNFFF+GOO+5AY2Mj/ud//ge9evVCVVWVKWOi1BQXSyiK1BQIK4pEcbG0YFT6yOJiSEXRFAhLRYEsjm/hGldVQgioAw+DsmcPhDxUZaLlxpkIVuRm3dOeKm/k8pwQEZF+ulaC77vvPnzyySd47LHH8Kc//QlSHgpQXC4Xxo8fj3feecewQXa2ZcsW1NXVYeHChbjhhhtw2mmnYfz48fjpT3+KXr16AQgHySeddBJ+/vOfo7KyErfccgsmTJiABx54wJQxUeq8XqCqKgiXK3Fw63JJ1NQE4bVzwrfXi0DVBEhXkr8p3W60T/gvdH0y3VaVkBLKV7shZGyViT6TxqNg+WOmPA07S1R5I1fnhIiI0qMrCH7jjTdw1VVX4YwzzoAQ8St5Q4cOxRdffJH24Lrz0ksv4cgjj8TZZ5/d7f2BQAAbNmyIW/GtqanBZ599hs8//9yUcVHqpk9vR7LFU1UFpk1rt2ZAaWiZPgNQk9Q0DoXQemNsC9eEVSW6PDxSZaJo7my4NzQaMGpn0FJ5I9fmhIiI0qcrCPb5fDjyyCN7vD8YDEa3Jhjtww8/xLHHHouHH34YY8aMwciRI3HZZZfhww8/BADs2LED7e3tGDZsWMzjjj76aADhlWSyh8rKEObPb4MQMm5F2OWSEEJi/vw2RzTMCFaOQfP8BZBCxK0IS5cbUgjg4YcRqhwTc5+mqhJdKS4ULs6dKhOa5ijH5oSIiNKna0/wkCFD8NFHH/V4/7p166JBp9G+/vprbN68Gf/+979x1113wev1YtGiRbj22mvx+uuv48CBAwCAkpKSmMdFvo7cnw632/lFNVwuJebfTLnuuhBGjmzFI4/kYdUqV7RjXE1NCDfe2I7KShVOKWISvO56+EaORMEjdchb9Vp032p7zQS0z/ghel0wDq6mTslckaoSGpPqIkQoCE/9Srjb2+K2VmQdjXPU3ZzY5Xs8V3C+rcc5txbn21pWzLeuIPiiiy7Cfffdh4qKClRWhhNShBAIBAJ46KGH8O677+LnP/+5oQONkFKipaUFv/3tb3H88ccDAEaNGoVx48bhqaeewplnnmnKdSMURaBv316mXsNKJSWZD6Kqq8P/+f1AUxNQUiLg9brhyK7e1eeH/+t4MqKkBB6vF56Ou2PmO9CMpPtBeiBUFX1dISCV78VDExwOFPfuBb78Ejj8cKC0VNc4TJfCHPU0Jwm/x7vOiVZ6H5cD7PA7Jddwzq3F+baWmfOtK8r4wQ9+gE8//RSzZ8+OrrDeeuut2L9/P4LBIC699FJcfPHFhg40oqSkBH369IkGwADQp08fnHDCCfj0008xYcIEAOEtG501NTUBAHr37p3W9VVVoqmpJa1z2IHLpaCkxIumJj9CIX2BmBk8HqC1Nfyf43mKgFYVaD3Y/XyHXOijsapEV1JRsD/kAvYdTHqsq/E9FDxch7z6leHVaQDIzwfa2iAASABy0CD458xFYOr1KY/FVCnMUdc5SfQ9HjcnioL2molovWlm3JYVIx6XC+z6OyWbcc6txfm2VjrzXVLi1bSCrCsIFkJEy6A1NDRg+/btUFUVQ4YMQXV1Nf7jP/5Dz2k1OeaYY7Bjx45u72tra8OQIUOQl5eHLVu24KyzzoreF9kL3HWvsB7BYPZ884dCalY9H7uLme+8fASqJoQrHiRrtNGJdLkRqK5BMC8fSPLaFSxbiqJ5cwDFdaiqAgDZEQBHvsbu3Si8bRZc762Db/HjKT8v02ico0Rz0vV7vNs5UVXkra5H3qrX0Dx/AVprp8ZdQ+/jcg1/p1iPc24tzre1zJzvlDda+P1+zJgxAytWrMCpp56K//mf/8Gjjz6KpUuX4ic/+YmpATAAnHvuudi/fz8+/vjj6G379u3DRx99hBEjRsDj8aCiogINDQ0xj6uvr8fRRx+dMKGPyGqaqkp0pYbQMm1G0sNSqjzR8V/+y39A/jJ7lRvTNEdGzEmCShOsUEFElH1SDoK9Xi/ee++9mO5sVjr//PNx4okn4oc//CHq6+vxxhtvYPr06fB4PLjiiisAADfeeCM2btyIn/70p9iwYQMeeOABrFy5EjNnzszImIl6krCqRJdjI1Ummucv0NQcQlflCQC9Ft6b8mPMpKXyhqFz0k2lCVaoICLKPrpS7r7zne/gb3/7m9Fj0URRFDz66KMYPXo0fvKTn2D27NkoKirC008/jQEDBgAATj31VDz44IP44IMPMHXqVKxcuRJ33303qqurMzJmokRaa6di/4oGBKprIJXwj6QUAuqgMkjR8bWiIFBdg/0rGrR95B6pqpDCNgsgvBqs7N4F7Nub6tMwVbdzZNKcRCpNdG5dretxRERka0J2bvem0c6dOzF16lRUV1fj8ssvx6BBg8wYmy2FQir27k2ejGR3breCvn17Yd++g9zbZAHN8+33Q/h84dbKXm/81xqJPXvQf+Qxusf77duNUE84QffjDaVzTrrOeapz8s3mTyEHDtT9OE10vr52xN8p1uOcW4vzba105ru0tJd5iXGTJk1CKBTCo48+ikcffRQulwsejyfmGCEEPvjgAz2nJ8pdXi9k52Co69cayeJiSL2VJwCoZZn/w9bduB6Fi+qidYKloiBQNQEtN87UtPWhq1TmRCpKODBN43GJGP3ciIgodbqC4PHjx3fbLpmIbMLr1Vd5AoA6qAzom9m6wT1VYvA0rIZn9Up9lRg0zkmk0kR0ZVbv46x8bkRElDJdQfCvf/1ro8dBRAZrmT4DntUrU37cwVm3mzAa7TpXYkA3lRgAoGjubASHj0h51VTTnHRTaULv47oy87kREVFq2PuPKEulVHmi47+2KReh7ZrMrkKaWYlBb6UJoypUsMoEEZF96A6Cm5ubUVdXh4suuginn346Tj/9dFx00UWoq6tDc3OzkWMkIp26raoAAPn50UA4sgXCN3+huY0y/H6IPXsSV0+woBKD3koTKT+u6/NllQkiIlvRtR3iq6++wpVXXonPP/8cw4YNwymnnAIA2Lp1K+rq6vDqq6/i6aefxkCtGdJEZJpgRSWaKirjKxHs2wtl1+5wEpyJe4BTSQITPp/mZD6hquHnoyNxsMc5MeBxPT1f/2Xft+S5ERGRNrqC4Pvuuw/ffPMNFi9ejLPPPjvmvnfeeQe33HIL7r//fsyfP9+QQRKRAbpWmuhbCtXkBLhUk8DMqMSQkM7qGz09LuHzrX8NUojwfuAkDHluRESUkK7tEO+++y5+8IMfxAXAAHD22WfjqquuwjvvvJP24IjIuXS1Gu6oxNB1321X0uVGoGairWrrJn2+ACAlpCvxnmA7PjciomykKwj2+/3o169fj/f3798ffu5nI8ppepPAWqbPANRQ4sdpqMRgNa3PFyHnPTciomykKwg++uijsWrVKgQCgbj72tvbsWrVKhx99NFpD46IHCqNJDCjKjFYSuvzVUOAEM56bkREWUpXEHz99dfjww8/xMUXX4znnnsOGzZswIYNG/D73/8eF198MTZt2oQbbrjB6LESkVZaKjGYeC49CW6d6a3gkDKD5iml5yslDjzxe/OfGxERJaQrMa66uhp+vx/3338/7rrrrmj3OCkl+vXrh1/96leoqqoydKBElJyR7XjTOZcRCW56KzhokemWzO1nn4P2qmpTnhsREWkjpNSQqtyDYDCIzZs348svvwQAHH744Rg5ciTcbl2xtSOEQir27j2Y6WGkze1W0LdvL+zbdxDBoLYVLNLPivmOqUzQ6WN56XIDaiildrxGnKuk9krNrYabHn9K07hS0dOcGzlPnZXUXgnP6lUQsufXVwoFgQkTTXm+mcbfKdbjnFuL822tdOa7tLQXXK7kmx3S6hjndrsxevRo1NTUoKamBqNHj87qAJjIrnRVYjD5XHZMcDNynrpqG3sOkCAABgBIFW1nnZvyuYmIyHi6guCVK1di3rx5Pd7/3//936ivr9c9KCJKjZHteI06lx0T3MxsW5y/9m1ASfIrVVGQ/+5bKZ+biIiMpysIXr58OTweT4/35+fn44knntA9KCJKgZHteA1u7WtZgpsWZrYtjpw7yZ5goapsiUxEZBO69i5s3boVF154YY/3H3/88Vi1apXuQRFlHb8fCDQDIReQl2/oqY1sNSy+3mN4a99ogpveNs1aksc0HGNmS2ar2j0TEZFxdK0ESynh61LSqLOmpiYEg4lXW4hygbtxPUpqr0SfwYcBgwahz+DDUFJ7pa49pz2JVCbQdGwPlRgi4+z3Hydpv67G1r6Rc/cfPgz9zqlE/+HDNM1B9HHlZeg/8hj0Ly+Le5yWY6LjNWCeejzexHMTEZE5dAXBJ5xwAlauXNlts4xAIIDXXnsNw4cPT3twRE5WsGwp+kyuCldI6FglFKoKT8Nq9Jk0HgXLHzPmQpFWwyLxj7MUSrfteGPGqbFYjNbWvnrnQMvjkh3jWbY09qSReUoSrEql+3lKyMHtnomIcpXuZhmffPIJrr76arz55pvYuXMndu7ciTfeeANXXXUVPv30UzbLoJxmZhWC7uitTJBonAlpqOqgdw40Pe72WSiaOzvhMYW3zgLWrYu5r23sOUCybQuqvgoOdqyGQUREPdMVBJ999tn45S9/iU8++QQ333wzLrjgAlxwwQW4+eab8emnn+IXv/gFzjnnHIOHSuQcZlYh6I7eygSaxtlJKlUd9M6B5jF1NOnpkcsFLFwYc1P+2reBJCvmEPoqONixGgYREfUsrWYZzc3NWLduHXbs2AEAGDJkCM444wwUFRUZNkC7YbMMSsrvR//yMs3dw77ZuiuthC/d10vhcZHHBmomomXajOSBnEVjSkpRsG/nVwjm5ac2JiHwzT+3ppbA18G9oRGFi+vgqV95qBud1nlzMP5OsR7n3Fqcb2tZ0Swjrc4WRUVFGD9+fDqnIMo6RlQKSKWtr97rpfI4APj2/zZBDhmi7ToWjSkpVYXwNQGlA1Ibk5ToP3yYrlbKZrZ7JiIi46TVMS5i/fr1uOOOO3D99dfjnnvuwRdffGHEaYkcKd1KAakmk+m9XsqPGzBA07FWjUkTRYEsLtF17rSTGL1eyIEDGQATEdmU5neEBx98EKNGjcLevXtjbn/hhRdw7bXX4qWXXsK7776LJ554AhdddBE+//xzwwdL5AhpVArQlUym93pmVjTwetF+2hgk22slAbRXnp76mBDespDwGLcbmDIl5XN3ZkYSIxER2YPmIHjDhg0YO3YsSksP7ZFrbW3FPffcg5KSEjz55JP461//igULFqClpQWPPPKIKQMmcgK9lQL0JpPpvZ65FQ20phvEHqdpTACQLJ0hFAJmzdJ37q4MTGIkIiJ70BwEb9u2DSeccELMbevWrUNLSwumTp2K0047DYWFhaipqcHkyZOxfv16wwdL5BS6KgWk0dZXb2UC0yoa+P3I+79GJKnfAAEgr3F96s/l3oVovndhwmNa7lsInHGG5uebcJx6WikTEZGtaQ6Cm5qaMKDLnsANGzZACIFzz42tqTlixAh8/fXXxoyQyKFaa6di/4oGBKprontRpaIgUF2D/Ssa0Fo7NeZ4Pclk6Vwv3cclHJ8FzyXZMYFrrkv+fJOVWUsyTiIici7NSyGHHXZYXMLbX/7yF5SUlOCYY46JO97LZBCiaKUAd3sb+rpC2B9yhct1dSOSuKW1rFh3rXf1ViYwuqKBVc8l0TGJfrlFH7dvL/oPH5bWOImIyJk0rwSfeuqpePHFF7F7924AQGNjIz7++GOcc845EF1WU/71r39h0KBBxo6UyMm8XuCwwxIHlkYmqumtTGBURYM0WzmnPCa94+5bynbHREQ5SnMQPGPGDPj9fpx//vk4//zzcd1118Hr9eKmm26KOS4YDOL111/HaaedZvhgibJdNrXe1dvK2WrZNOdERKSd5iD4iCOOwIsvvoiLL74YQ4cOxfe+9z288MILOOqoo2KO27hxI0aOHImJEycaPliibJdNrXf1tnK2WjbNORERaZdW2+RcxLbJpEeq8+341rtGto7WKefmPMP4O8V6nHNrcb6tZfu2yURksI4Er+BJo9D0+FPGtt61sI2vEa2j0+b3A4FmIOQCekhG7KxzspyyazfUskFA39KkjyMiImcysD8pEenlblyPktor0b+8DP1HHoP+5WUoqb0S7k0fpp2o1uO5TeyAlm7r6HREnm+fwYcBgwahz+DDND3f6DwNH4Z+51Si//Bhps8TERFlDoNgogwrWLYUfSZXwdOwOrp6KlQVnobV6DNpPAqWP2bLcydkZHWIFOh9vhmbJyIiyhgGwUQZ5G5cj6J5cyCkjOsUJ0JBCClRNHe2rtVIM8+thdXVIfQ+30zPExERZQaDYKIMKlxUByiuxAcpLhQurrPVubWwujqE3ueb6XkiIqLMYHWIFLE6BOnR7XybWUEh09UZrL6+3utlep6yCH+nWI9zbi3Ot7VsWx3ilVdeSXpMfn4+Bg0ahBEjRsDj8ei5DFFWM7OCgvh6T0arM1hdHULv9WxRxYKIiDJCVxA8b968aKvkrgvJnW8XQqCoqAg33HADrr/++jSHSpRdIhUUtK5Caqmg4G5cj8JFdfCsXql9HACK5/wQLTNuMawerhnPzYzrWT1OIiKyD117gl955RUcd9xxqKiowIMPPohXX30Vr776Kh544AGcdtppGD58OJ599lk8+OCDOOGEE7BgwQI888wzRo+dyNkiFRRcif8WlS63pgoKMRUOUtjlJAB4/vS6sVUQrK4OoXcuDX4NiIjIOXQFwU888QT69euH5cuX4z//8z9x3HHH4bjjjsMFF1yA5cuXo2/fvvjDH/6A888/H8uWLcOoUaPw7LPPGj12IsdrmT4DUEOJD1JDaJk2I+EhiSocaGFGFQSrq0PonUujXgMiInIWXUHwn/70J5x33nnd3ieEwLhx4/DHP/4xfAFFwfjx47F9+3b9oyTKUsHKMWievwBSiLjVSOlyQwqB5vkLkm5T0FThQAsDqyBYXR1C71wa9RoQEZGz6AqCVVXF1q1be7x/69atUDvtsfN4PMjPT962lCgXtdZOxf4VDQhU10S7rElFQaC6BvtXNKC1dmrsA/x+iD17wm2BO772rFmlawW4KxEKwlO/8tC59YqMKcleW6GqxlyvQ8xcRvIThOh5Lrt7nJ7XgIiIHEdXYty4cePw7LPP4qijjsLFF18cDXDb2trw/PPP4/e//z2qq6ujx//tb3/DkCFDjBkxURYKVlSiqaIyHFz5fOEErC77T6NJbx3BpVQUBKomwH/Z9zVXONDCymoNEsC3fftiZ6sfhYWFKJUSQvdVIyeVgCoBIcL/X4jw11ofF9lPHfm6k55eg5YbZ3KlmIjIYXTVCd6/fz+mT5+OjRs3Ii8vDwMGDAAAfP3112hvb8dJJ52ERx99FH369EFbWxt+8YtfoLKyEhMnTjT8CViNdYJJj3Tnu2DZUhTNmwMorpgVX+lyA6EgIERKyXCJWFG3d3/v3njiBz/AgzNn4rNjjonePjSk4jp/AJe2tqO3jqeTcJ7UEJrnL+h2NVjL4yClrnPnCv5OsR7n3Fqcb2tZUSdYd7MMKSX++Mc/4s9//jO++OILAMARRxyBM888E+effz6UZHsBHYpBMOmRzny7G9ejz+SqhEGuBACXCyLUc4JX5NGJVlolgPbTz8CBV1anNMbulNReGa5U0WWbRsMFF+DCF19ES2Fh+JqdfldEnqMXwOMH/BjXniRhrRNN8yQE9q9oiFm11Ty/SDJ33Zw7l/B3ivU459bifFvLts0ygHAC3AUXXIALLrhA7ymISINo0luiPb+KC0gQAKcm7Q0JAMJVF7rWK2644AJMWLUqnITWzR/KkX28rVLiyt5ePJ1CIKx1ngoX14W3nqTyOODQ9ooUzk1ERPaVncu1RNlCY9KbUEOAED1XOADCWyaSXE4AyGt8z5CEr65VF/b37o0LX3wRUgiorsSVLFQhIAFc29uLA1picq3z1DXxT+vjgKTbTQxLKiQiIkvoWgmWUuK5557DH/7wB+zcuRNNTU1xxwgh8I9//CPtARLlspTa+kqJ/U8+B+/zT8NTv/JQ4lZ1DfyXXIk+V1+q7TxdE+MSJOsl01o7FcHhI1C4uA7Ljy5HS2FhtyvA3VGFgF9KPF+Qh+v97YnHbEHb5FTPTURE9qYrCL733nuxfPlyDB8+HJMmTULv3r2NHhcRIfW2vu1nn4P2qur4wNXvT7k9sFGVEIIVlThQUYnf9vGGtxSkaInXg+v87Yn346YyT+jUJvqkUZofpwVbKxMROYeuIPiVV17BBRdcgN/+9rdGj4eIOuto69tdglln0uVGoLomph1wzGpkiucp+P3ThyohdASIQlXhaVgNz+qVKVdC2CsEtuWl/utGCoFtLoF9AihNtBsh8vy01CZGuE205/XVaJ6/QNu8AEkrcMS9BkREZGu69gS3trbi9NNPN3osRNQNo9r6aj1P25nn9NiCWW975YNp5to1a1hBbht7DqB1S0Sn59E29tzk8wIkTooD2FqZiMhhdAXBY8aMwd///nejx0JE3TCqra/W8+SvfSt5C+YU2yv3SrOEcZGGSo75a98GRIq/0hQX8v/8dvJ5uXchmu9dyNbKRERZRFcQfNddd+HDDz/EokWLsG/fPqPHRERdpNzWV+95Lr1CX5WFJEqlxNCQmnJDD9HxuL7JHhap8iBT29sbeR6tl16RdH6Neg2IiMgedDXLOPnkkyGlRFtbGwAgPz8/rjmGEAIffPCBMaO0ETbLID0Mne80qjXE2LcXyq7dUMsGAX1LAQBizx70H3lMkgce8s3mTyEHDtR07KNu4M4+RdFawFoIKXH3wbbk1SFSHHdX37y/GTLS2r2beYmj5Zgcw98p1uOcW4vzbS3bNssYP348hI4sbyIyQNektxQlrPqQQrUErZUQIte7af2fcc+OHfB7vUnrBAOAIiUKAFzSmjgABlKrDtGdfqedhPbTKgEhkLdhfY/VMIyqmEFERJmnu21yruJKMOlhl/kuWLb0UNWHTlsepMsNqCE0z18Az9tvaq4i0fT4Uyldr3PHuESBsCIlBIBnDvhxrsaOcT21adaqu9bInecFUiadu1zeEmGX7/Fcwjm3FufbWlasBLNjHFGOcDeu11T1QVO1BA2VELq73vjXX8eqCRPg9fvDjSW6rNwKKSE6VoBTCYABjdUvEhCIbxgdnZfbZ6Fo7mxDK2YQEVFmadoO8corrwAAJk+eDCFE9Otkvvvd7+ocFhEZrXBRXbjqQ6KV0k7VEormzk646pns4/+erjf+9dfx+ZFH4smrr8YDP/whPjvm0F7eo1SJ6/0BXNrajpIUP6OKVL/odtyID3BTJkTiMmkdFTOauC2CiMgRNG2HOP744yGEwIcffgiPx4Pjjz8++YmFwMcff2zIIO2E2yFID3d7G/q6QtgXciGYl2/9APx+9C8v07zX95utu+De9CEKF9fFtmCumYiWaTOS73/VeD0J4Nv+/bF948coys9HX5l+sOre0Bge96rXIKSEBKAOKoOyezcEzN39FZm7XGyYwd8p1uOcW4vzbS3bJMa98cYbAACPxxPzNREl1jmRCqqKPhlKpBI+n+akMaGqED4fghWV4VVNHdUotF5PAOj/zTfA/gOaq0wkJSWgykMrt4qC4Cmnwv+9i9HnuquNuUYPInOXTuIiERFZQ1MQfMQRRyT8mojixSSFGdB6OB2pVE+Iq/qgoxpFWtdLQ8I5r38NMknr43QZ+VyIiMhcTIwjMoHWJDTLEqm8XrSfVpl0M4AE0F45Jv2P871etJ82RuP1Tjdk+0DSOQcAKSE1lGfrbpzJ6htLlxuBmok5uRWCiMiJNK0EX3116h8hCiHwxBNPpPw4omygNQnN2kQqrbttjaoBrnXF1ZiVWa1zjpDOChLJVpA1VMwgIiL70BQEd5c7t3v3buzcuRPFxcUYPHgwAODzzz9HU1MThgwZgkGDBhk7UiKniLTwTbIVIKb1sNmrh34/8v5vfdLwVgDIa3wv/TH5/cj7v0aN11tvyPU0zbkaghQd6XHdVJCIjCl6W+c6wUDaFTOIiMg+NAXBv/vd72K+fv/993HTTTfhF7/4BaZMmQK3O3yaYDCIl156Cffddx/uuece40dL5AB6ktBS2nOrJVGtyzGmj6nrOcy8XjfPP6XrSYn9Tz4H7/NPx1S+aK8cA0Agr/G9Q9UwqmvQUns9QscPhywuRnD4iPiKGdU12ipmEBGRrehqm3zvvffie9/7Hi6++OLYk7nduOSSS7Blyxb8+te/xgsvvGDIIImcxKykMC0te3s85tobLE1UM2MOjGz33H72OWivqo4JqN0fbgxvqYh88qWqcP/l/9Bn1UoIGXu9poeWpFwxg4iI7EVXYty//vWv6BaI7hx55JH497//rXtQRI7m9SJQNSH8MXkCqSRSFSxbij6Tq8JtgbtUPegzaTwKlj+W+JiLJyF4wghDx5RQZA5E4l8xUiiarpf0+T/3jL4593ohBw5Ewe+fPnT+jiBYAFC+2g0hu7+eHDiQATARkYPpCoIHDhyI+vp6BIPxCSjBYBD19fUYaFTNTyIH0tTCV2MilaZKExra+ro3/z1x0lgKY9Kibew5gEyyMitVtJ11bsJDzG73nPD8XR7OFslERNlDVxB83XXX4YMPPsAll1yCF154ARs2bMCGDRvw/PPP45JLLsHf/vY3TJ1qTf1TIjuKtPCVQsStTkqXG1IIzYlU0aoHySQp4QWXG8ETTzJkTFrkr30bUJL8ilEU5L/7VsJDND3/Tu2eU31+mue3y/UKF9el9hgiIrIVTW2Tu/PCCy/gN7/5Db799luIjjdfKSVKS0txyy234JJLLjF0oHbBtsmUimgLXz2th4GU2h1rIRUF+59/FYXLl+gfkxY62jR3u7XA7HbPacxvLrdI7g5/p1iPc24tzre1bNM2uTsXX3wxpkyZgs2bN+PLL78EABx++OEYOXJktFoEUa6LtB52t7ehryuE/SEXgnn5mh8vvt5jWAAMhPe1ho4fjqbHn9LVDlnzdQyqDpFOu2ctc57K+VMZNxER2V9a0arb7cbo0aMxevRog4ZDlKW8XqBvL2DfQUDDX7TRSgirVxo6jJhKDDraIWu+jkHVIdJt95xszlM5fyrjJiIi+0srCP7000+xc+dOHDhwoNv7v/vd76ZzeqKcVLBsKYrmzQk3ZdC4W0kCgBAJj5cuNwLVNdZ8fN9RHcLTsDou2SylMRl1njTHadj1iIjINnQFwTt27MBtt92GTZs2ddtNDgi3TWYQTJSazpUKklZy6MpmbX1bps9IvpKtYUxGnSet8xt4PSIisgddQfBPfvIT/Pvf/8Ydd9yBU089FSUlJUaPiygnRSsVaAyA7dzWN1IhI90xGXUeXedHz22U2SGOiMjZdAXBf/3rXzFt2jRcddVVRo+HKHf5/dFuaFp017LXbm19W2unGjImo86T0vmFgHrYIChffXWoYxxbJBMRZQ1dQXDfvn1RzIQQIkOlWqng2//bBDlkSMxtkcoIZlZ+SFV0TPv2Qtm1G2rZIKBvqf7zmPTcejy/jeaSiIiMo6tZxmWXXYYVK1YgFErSnYmINItUKtB0rKJADhjQ8wEd7YDtELS5G9ejpPZK9B8+DP3OqUT/4cNQUnul/o5rZj+3rue30VwSEZFxdK0EDx06FKqqYvLkybjwwgsxaNAguFzxHZcuuOCCtAdIlDPMroSQATGVLjpWuYWqwtOwGp7VK9E8fwFaa9ldkoiIrKcrCJ41a1b0/8+fP7/bY4QQ+Pjjj/WNiihHmV0JwUqJKl1EgvyiubMRHD6Ce2yJiMhyuoLgJ5980uhxEBHMr4RgJU2VLhQXChfXhffiEhERWUhXEHzaaacZPQ4i6mB2JQRLaKx0IUJBeOpXAn6/fbZ3aEmEM+oYIiLKmLQ6xgUCAXz00Uf49ttvccopp6C0NPWMbyKKZ8cqD6lIpdKFUNXwc8zw84u2qu4I3qWiIFA1AS03zoz+4WHUMURElHm6g+Ann3wSdXV18Pl8AIDHH38cY8aMwd69e1FdXY3bbrsNF110kWEDJcpJXm/Gg0M9IpUutATCUlHCQX4GaUngg5SGHMNEQCIie9BVIu3FF1/Er371K5x11ln45S9/GdM6ubS0FJWVlaivrzdskETkMB2VLqQr8d/Z0uVGoGZiRle5Oyfwda3KIUJBCClRdPssFM2dnf4xc2frLw1HRESG0hUEL1u2DOeddx7uv/9+nHvuuXH3jxgxAp988knagyMi52qZPgNQk9QSt0Gli2gCXzJCpH9MRyIgERFlnq4gePv27Rg7dmyP9/fp0wf79+/XOyYiygKRShdSiLgVYelyQwqR+UoXkQS+RBUsAAggXOot3WM6JwISEVFG6QqCS0pKsG/fvh7v//TTTzEgUTcrolyzdy+weXP4Xzvz+yH27DEsSGutnYr9KxoQqK6JdsOLVLrYv6LB3P2xfj/w1VeHnks3z018vSelVtVGiCQCEhFRZukKgseOHYvnn38eTU1Ncfd98skneOGFFzBu3Li0B0fkdPmPL0HpiceizzFDgBNPRJ9jhqD0xGORv+yxTA8tRrS1cXkZ+o88Bv3Ly9JrbdyZlIAqw/92/tokkefSZ/BhwKBB6HPkQJSeeCz6Dz303HpPqkLvydXo9x8nmTaOntghEZCIiAAhZZLP77rx1Vdf4ZJLLoGUEueeey6ef/55TJo0CaFQCK+//joGDBiAF154IStLpoVCKvbuPZjpYaTN7VbQt28v7Nt3EMGgtSthuaL4hmuQ/8qLAMIflUdEfuDaplwE3+LHLR9XVzGVEXpozqF3xdbMc6d0PXT/GmjY5auZBAAhEm6JiLS8bnr8KQOvbA/8nWI9zrm1ON/WSme+S0t7weVKvs6rayX4sMMOw0svvYSzzjoLq1evhpQSr776Kt566y1MmDABzz//fFYGwERa5T++BPmvvBjeJ9rlvsht+S//IeMrwpoqI+isaGDmuVO+Xpdju3tdDJFsTcEGiYBERBSmayW4q71790JVVZSWlkJRdMXVjsGVYNKi9MRjoXy1O2GgJQGog8qwd9O/rBpWnJLaK+FpWJ0wMUzv6qWZ59Z7PTN0XtUGkLTldbbWCebvFOtxzq3F+baWFSvBaXWMAwApJaSUEEJAaCkhRJTt9u1NGgAD4ZVIZfcuYN9eoG8GPjkxs7Wx1W2TNV7PaN21s3Z8y2siohyhOwj+9NNP8cADD+Ddd99Fa2srAKCgoABnnXUWZsyYgWOPPdawQRI5ibIreQAcITqOV3sKgk1sm2xma2Or2yZnosoDAHz7f5sghwyJuc3pLa+JiHKFriD4/fffx/XXXw9VVXHeeedh6NChAICtW7fizTffxNq1a7F06VKceuqpRo6VyBHUskFxiVg9kR3Hd+VuXI/CRXXR1U2pKAhUTUDLjTMNW000s7WxVW2To/O0eqWux6dDKgpkolKQDm15TUSUK3QFwb/61a9QWlqKp556CmVlZTH37dq1C1deeSXuuecevPjii4YMkshR+pZCPWyQ5j3BXbdCxFQ46AgiharC07AantUrjdtX2tHaWOu+3ZRWMyPnXr0KQvYcCEuh6G6bHDNP6ac2HBoToLnKA1d4iYicS1cW26effoorrrgiLgAGgLKyMlx++eX49NNP0x4ckVMdnHWbxuNuj/na6ooKZrY2bht7DpAgAAYASBVtZ8W3Xk8m0TwZglUeiIiynq4g+PDDD0cgEOjx/vb2dgwaFP8RL1GuaLv2erRNuQgSh2rSRkRua5tyEdquiV3RLVxUByiuxCdXXChcXGfIOM1sbZy/9m0gWbUYRUH+u2+lfG5N89RJSuvEQkHwxJPs3e6ZiIjSpisIvvnmm/G73/0OH3/8cdx9//jHP/DUU09h5syZaQ9Oi4MHD2Ls2LE47rjj8Pe//z3mvhdeeAHjx4/HiSeeiEmTJuGtt1J/s7W9oARaZPhfshXf4sfhm78Q6qCyaBAW2QLhm78wvlFGpMJBkpXNmIoKBjCltbHW6hCqmvpz0ThPEVKIjtdAW7qikCrcH23G/hdWpDcnBregNo1TxklEZDBde4I//PBD9OvXD9/73vdw8skn46ijjgIAbNu2DRs3bsT/+3//Dxs3bsTGjRtjHvfjH/847QF39fDDDyMUiv84d9WqVbjzzjsxffp0VFZWor6+HjNmzMDTTz+N0aNHGz4Oy+2SUDaFILYBQgJSAHIooJ7kAspYqs4u2q6ZirZrpsLdtB99/Qew39sbwZI+3R5rdUWFzoyuaGCXyhMRwVNOhf97F6PPdVdrHlPo+OHh+sUpzokVSY1GcMo4iYjMoqtZxvHHH5/6hYToduU4HZ999hkuuugizJ07F3fddRf+8Ic/4MQTTwQAjB8/HiNHjsT9998fPf6yyy5DcXExlixZovuadmiWIT5SobyrAiIcAEdIAUAC6lkK5IjEi/ws+m0tTfPt96N/eZnmigrfbN1l38QsM59LCueOXsPlBkLBpAlvusfUweo20XqZMU7+TrEe59xanG9r2bZZxj//+U89DzPc3Xffjcsuuwzl5eUxt+/cuRPbtm3DbbfFJifV1NTg3nvvRSAQgMfjsXKoxtklobyrhj/Y7fJeHgmIlXdVhEoFV4SdxsxqDVazovJECt3hIsdJKSFdLohuPj1Ka0yITdZDN0mNQLibXHD4iIyutDplnEREZnNsj+M1a9bg3//+N26++ea4+7Zs2QIAccHx0Ucfjfb2duzcudOSMZpB2RRKXoBWdBxHjmNmtQarmflcNJ27O4oLSBAApzMmq5Ma9XLKOImIzJZ222QgvC1hzZo1+Prrr1FeXo4LL7wQRUVFRpy6W36/H7/+9a8xa9asbq9z4MABAEBJSUnM7ZGvI/fr5XZn6G+HoAS2IWmqu5CA2AYoEIC7+4g58jGBlo8LKH2a5/vMM9By30IU3joLcLkggp0+qna7gVAILfctBM443ZgfXjOZ+VwSnDsRoYbCVR8AY8eUYptod3tbZlbyTRwnf6dYj3NuLc63tayYb82/55966in87ne/w7PPPovS0kPF/d9880386Ec/Qnt7e8yxzz33XMxxRnrkkUfQr18/XHjhhaacPxFFEejbt5cl15LtErJNQuQLiDwBtVnFAakxgJdAb28hlKLE3zwlJTb+SD0LaZrv2T8CKk4FFi4EXn4ZUFVAUSAmTwZmzUKvM86ANd+BBjDzuXQ+90svJa/t20FICaxYATzxhHFjCjSHz6Pl+qqKvq4QYNHvkRgWjJO/U8zhb/ejqa0JJfkl8ObFzjHn3Fqcb2uZOd+ag+A333wTgwcPjglsg8EgfvzjH8PlcuHnP/85Ro4cibfffhu/+c1vsGjRItxxxx2GD/iLL77A448/joceegg+nw8A0NLSEv334MGD6N27NwDA5/NhQKe2pk1NTQAQvV8PVZVoamrR/XhNvlSBjSFgq0S0/265AE5Swv9fy3u9AA74W4D2nleCS0q8aGryIxTiBn+zpTzfJ4wGljwBPLAIwtcEWVxyaEVuX2YTM1Nm5nOJnHv+QvQ5dqjmRLz93xkDnDnOuDGFXOiTQpvo/SFXZl5HE8fJ3ynmaPzyPTz8tzrUb1kJVapQhIKaYRNx08kzccbgMzjnFuL3uLXSme+SEq+xiXGffvopLrnkkpjbNmzYgL1792LatGmYMmUKAOD//b//h3/+85945513TAmCP//8c7S3t+OGG26Iu+/qq6/GqFGjohUhtmzZgmHDhkXv37JlC/Ly8jB48OC0xmBmVmi3lR8kILdKYEsI6Adgb2xViK6i5dKQvH5wKKQyy9VCKc93Xj5Q2vGHnNNfJzOfS0mflBLxgnn54TEYNaa8fH3Xt5oF4+TvFOMs27wU89bOgSJcUDu6L6pSxeot9Vj12Wu479yFmD32R5xzi3G+rWXmfGsOgvfv3x/XBW79+vUQQuA///M/Y24/5ZRT8Mc//tGYEXYxfPhwPPnkkzG3ffzxx7jnnnvws5/9DCeeeCIGDx6MoUOHYs2aNTj//POjx9XX12PMmDH2rQyhofKD/FbDeWRHvWCiHNIyfQY8q1cmPsjEpMJMX18rp4wz1zXuWo95a+dAQiIkY/9giXx961uzUDH0VJxQPDoDIyRyPs27jfv3749vvvkm5rb3338fBQUFcXWDPR4P8vLyjBlhFyUlJaioqIj5b/jw4QCAESNGYMSIEQCAmTNnYuXKlXjggQewYcMG3HXXXdi0aRNuuukmU8ZlBK2VH2S/jta7XY6VoqMj2VkKy6NRzjGzBbQTrq+VU8aZ6xZtrIMiEi9muIQLCxsXWjQiouyjOQgeOXIkXn75ZTQ3NwMAPvnkE/z973/HWWedBbc79hfpli1b4laNrTZx4kT84he/wMqVKzF16lT89a9/RV1dHU4++eSMjqtHQRnt/paIkIDYC4QmKpBDDwXCkS0QocmupI0yiLKVKS2gHXR9rZwyzlzlD/qxZtuquBXgroIyiJf/+TL8Qba8JtJDc8e4f/3rX7joootQUlKCY445Bh999BFaW1vx+9//HiNHjow59vzzz0dlZSXuvvtuUwadSaZ1jGuRcD+pve5p8GoXUCjCe34DADzosRxad9j5xlqcb+u529vQ1xXCvpArvLfVaga1oDadQePk97hx9rTswcjlx2g+/p/XfYZSz4DkB1Ja+D1uLSs6xmleMjzuuOPwxBNPYMSIEdizZw9GjRqFRx99NC4A3rBhA7xeL6qqqlIacM7zxG9v6IkU4eMBhAPfwp7rARPlLK8XOOywzAWgXi/kwIH2DoAB54wzhxR7iqEIbW/PilBQ7ClJfiARxUmpHvwpp5yCRx99NOExFRUVeO2119IaVE5yC8ihALZpq/zAoJeIKDt53V5UDZ2Ahm2rE26JcAs3Jh8/GV63lyuTRDpw86iNqCe5ktcAZuUHIqKsN330DKgy8Ra5kAxhVuUsi0ZElH0YBNtJmYB6lsLKD0REOa6ybAzmj10AAQGXiP3Q1iXcEBC479yFOGPIGRkaIZHzpbQdgswnRygIlYpwubRt4a0R0eYXJ7kYABMR5YjakVMxvN8ILP6wDvVbD3WMqy6vwbRRM3DG4NMzPUQiR2MQbEdlAmqZW3flByIiyg4VZZWoKKuEP+iHL+BDsacYXjeTGImMwCDYztyCrxAREcHr9jL4JTIY9wQTERERUc5hEExEREREOYdBMBERERHlHAbBRGYISqBFhv8lchB/0I89LXvgD/ozPRQiIlMx7YrISLsklE0hiG2x5e1wihvom+GxESXQuGs9Fm2sw5ptq6KluKqGTsCNo2eioqwy08MjIjIcV4KJDCI+UuF69VAADIT/FdsAvBhE2wdtGRwdUc+WbV6KyS9XoWHbaqgy3H5XlSoatq3GpJfHY/nmxzI8QiIi4zEIJjLCLgnlXRUChwLgiMjXLfUtwC7V8qERJdK4az3mrZ0DCYmQDMbcF5JBSEjMXTsbG3Y1ZmiERETmYBBMZABlUwhI1s9EAbAxZMVwiDRbtLEOinAlPEYRLiz+sM6iERERWYNBMFG6gjJmC0SPVABbmCxH9uEP+rFm26q4FeCuQjKI+q0rmSxHRFmFQTAdoqeiAasgAC0yeQAcIRFuhU2UCr8fYs8ewG9sEOoL+KJ7gJNRpQpfwBceDitIEFEWYHUI6rGigXqSCyjr4TN+PY/JNpE52JrCYwQAj1kDomzjblyPwkV18KxZBaGqkIqCQNUEtNw4E8GK9Cs2FHuKoQhFUyCsCAX/3PsP3P7OrG4rSJwx+PS0x0NEZCWuBOe6v4d6rGjgejUE8VH8m2OiKgg9PSbbxMyB1gcpAIYJwJ0jfyRQWgqWLUWfyVXwNKyGUMM/U0JV4WlYjT6TxqNgefoVG7xuL6qGToBLJF4PcQk3Tug3AhevmNxjBYllf1+a9niIiKzEIDiHBXcEgXdCPVY0EACUd1VgV6c7k1RB6PYx2SbBHCSkAhidOAGJCAivABfNmwMhJUQodr+uCAUhpETR3Nlwb0i/YsP00TOgysQJmyEZxOZv/p6wgsStb83Cuh3r0h4PEZFVGATnsNbG1uTLmKKj8kEHTVUQujwm22iag05kx7GFNYVAGX/kKLnCRXWAkuQPJsWFwsXpV2yoLBuD+WMXQEDErQi7hBsCAiP7nahhtdiFhY0L0x4PEZFV+I6cq4IS7f9uDydqJRBt9hCUmqsgxDwm22itBNEh2jHuQjfyv5Nv4sAoa/j94T3AocQVG0QoCE/9SkOS5WpHTsWKKQ2oLq+BIsJvC4pQUF1egxcmrcA/9n6UtIJEUAbx8j9fZrIcETkGE+NyVQBJA+AI0amigdbgL/qYbPsOC6S2BSJ0uQKUKFDc/HuTtBE+X3QPcNJjVRXC54P0etO+bkVZJSrKKuEP+uEL+FDsKYbX7cWelj0pVpBoQqlnQNrjISIyG9+Zc5UHmj/Sl5GKBp5DH+1rfky2SXUOCpkER6mRxcWQirZfzRJA8ZwfGrI3OMLr9mJg4UB43eHAOlJBQgtFKCj2lBg2FiIiMzEIzlVugbxj85IGwtGP893hqgZyaPIgMOYx2YZzQGbzehGomgDpSv4xigDg+dPrhlWL6HY4GitIuIUbU46fEg2eiYjsjkFwDiuoLEi+JUJ21P7toJ7kSvkx2YZzQGZrmT4DULUllxpdLaI72ipIhDCrcpYp1yciMgOD4BzmHuIGznFBIn5lU4pwnKeepcQ2vygTUM9SUntMtuEckMmClWPQPH8BpBCaVoQBGFYtojtaKkjcd+5CnDHkDFOuT0RkBgbBuW6kC6HJrpiP+CMf5YcmuyBHxH+LyBFKyo/JNpwDMltr7VTsX9GAwAXjNeWwGlktojuJKkismNKAa068zpTrEhGZJdty90mPMgG1zB0uaRZAOKEt2V5WPY/JNpwDMlmwohK+8mHov3qVpuONrBbRnZ4qSBARORGDYDrELVL/jtDzmGzDOSATRapFaCmbJhUFsrjY9DF53V4Gv0TkePzMlojIzjRWi5AuNwI1EwGTVoGJiLINg2AiIpvTVC1CDaFl2gxrBkRElAUYBBMR2VyiahHS5YYUAs3zFyBYUZmhERIROQ+DYLKPoARaZPhfIooRrRZRXRPtKCcVBYHqGuxf0YDW2qkZHiHp5Q/6sadlD/xBcyp7EFH3mM5DmbdLQtkUgtgGCHmo1Jh6kou1dok6CVZUoqmiEvD7w1Ugiou5B9jBGnetx6KNdVizbRVUqUIRCqqGTsCNo2eiooyr+kRm40owZZT4SIXr1UMBMBD+V2xD+PaPkmfEE+Ucrxdy4EAGwA62bPNSTH65Cg3bVkOV4d9zqlTRsG01Jr08Hss3m9MGm4gOYRBMmbNLQnlXhcChADhCSEAAUN5VgV3cHkFE2aNx13rMWzsHEhIhGYy5LySDkJCYu3Y2Nuwypw02EYUxCKaMUTaFwpFuIqLjOCKiLLFoYx0U4Up4jCJcWPyhOW2wiSiMQTBlRlDGbIHoSWRrBJPliCgb+IN+rNm2Km4FuKuQDKJ+60omyxGZiEEwZUYgeQAcIWT4eCIip/MFfNE9wMmoUoUv4DN5RES5i0EwZYYnXAVCCwlAWRvi3mAicrxiTzEUoe2tVxEKij3mt8EmylUMgikz3AJyqLZAWAAQ21ktgoicz+v2omroBLhE4gqlLuFGTflEeN2sAEJkFgbBlDHqSa7wMq8GrBZBRNli+ugZUGXihF9VhjBtFNtgE5mJQTBlTpmAepYCCe1bI1gtgoicrrJsDOaPXQABEbci7BJuCAjMH7uADTOITMYgmDJKjlAQmuyCHKJtUZjVIogoG9SOnIoVUxpQXV4T3SOsCAXV5TVYMaUBtSPZBpvIbGybTJlXJqD2dsH9pLYV3mi1CH73EpGDVZRVoqKsEv6gH76AD8WeYu4BJrIQwwiyh45qEVrKpkkRPp6IKBt43V4Gv0QZwO0QZA8aq0VIAcih4eOJiIiI9GIQTLahqVqE7DiOiIiIKA0Mgsk+ElSLkCIcH6tnKUAZV4GJiIgoPdwTTKkJynBSmgembEmQIxSESkW4DNq28B7hyBYI9SQXA2DKekySIiKyBoNg0maXhLIpBLHNgsC0TEAtc5secBPZSeOu9Vi0sQ5rtq2CKlUoQkHV0Am4cfRM1oslIjIBt0NQUuIjNdyyeNuh6g2Rer2mtjJ2C6BQMACmrLds81JMfrkKDdtWQ5XhnydVqmjYthqTXh6P5Zsfy/AIiYiyD4NgSmyXhPKuCoH48mVsZUyUvsZd6zFv7RxISIRkMOa+kAxCQmLu2tnYsKsxQyMkIspODIIpIWVTKBzpJsJWxkS6LdpYB0UkrniiCBcWf1hn0YiIiHIDg2DqWVDGbIHoCVsZE+njD/qxZtuquBXgrkIyiPqtK+EP+i0aGRFR9mMQTD0LaOvgBnRqZUxhQQm0SP5hQAn5Ar7oHuBkVKnCF/CZPCIisgt/0I89LXv4x6+JWB2CesZWxqnroYoGTnEDfTM8NrKdYk8xFKFoCoQVoaDYU2zBqIgok1gpxjpcCaaesZVxShJV0cCLQbR90JbB0ZEded1eVA2dAJdIvB7hEm7UlE9k3WCiLMdKMdZiEEwJsZWxRkmqaABAS30LsMukcnLkWNNHz4AqEyeWqjKEaaNmWDQiIsoEVoqxHoNgSoytjDXRVEVDAbCRVTQoVmXZGMwfuwACIm5F2CXcEBCYP3YBPwYlynKsFGM9BsGUlByhIDTZFbM1IrIFIjTZBTkix7+NNFbRgApgC5PlKF7tyKlYMaUB1eU1UET450kRCqrLa7BiSgNqR07N8AiJyEysFJMZTIwjbdjKuGcpVNFApIoGf/Koi4qySlSUVcIf9MMX8KHYU8w9wCngvJGT6akUw+/z9PGtmFLjFvyu6SqFKhpgFQ1Kwuv28s0tBcykp2zASjGZkeOfYxMZQGMVDSgAhgmuoBMZhJn0lC1YKSYzGAQTGUBTFQ0VwOgcr6JBZBBm0lO2YaUY6zEIJjJCkioaAFBYUwiU8UeOyAjMpKdsw0ox1uM7MlEqErRDTlRFAxe6kf+dfCtHSpQ1uraPZSY9ZStWirEWU5yItOihHbJ6kiu2RnIPVTQUN//eJEpVT0lvlx3/fWbSU9ZipRjrMAgmSkJ8pEJ5VwVEbDtkbANcW0PhbRBdayWzigZRWpZtXop5a+dAEa64pLf6ra9BQEAm3YjPTHpyLlaKMR+Xp4gSSdIOWQDhAHkXG2AQGSVZ0hsASEi4kuwJZiY9ESXCIJgoAU3tkEXHcURkCE1Jb3AhxEx6IkoDg2CinmhshywkILaB7ZCJDKA16U1FCKLjf8ykJyI9GAQT9SSFdsgi0g6ZiNKSSvtYCYknqn/PTHoi0oWpO0Q9SaEdsmQ7ZCJDpNo+9uzB56CqvJqZ9ESUMq4EE/VEYzvkaC1gtkMmSpve9rFetxcDCwcyACYizRgEEyWgqR2y7DiOiAzB9rFEZAUGwUSJJGmHLAGoZymxDTOIKC1sH0tEVuCeYKIk5AgFoVIRLoO2LUnHOCIyRO3IqRjebwQWf1iH+q0rox3jqstrMG3UDAbARJQ2BsFEWvTQDpmIzMP2sanhPBGlhkEwUSrYDpnIcmwfm1jjl++h7oMHsWbbquiKedXQCbhx9EyumBMlwD3BREREDvXIXx7BhD+MR8O21dGycqpU0bBtNSa9PB7LNz+W4RES2ReDYCIiIgdq/PI93Fx/MyRkXIe9kAxCQmLu2tnYsKsxQyMksjcGwURERA708N/q4FISl2dUhAuLP6yzaEREzsIgmIiIyGH8QT/qt6xEUA0mPC4kg6jfuhL+oN+ikRE5B4NgIiIih/EFfJpaSwPhPcK+gA9AOHje07KHQTERmOdORETkOMWeYihC0RQIK0LBP/f+A7e/M4sVJIg64UowERGRw3jdXtQMmwi3kngtyyXcOKHfCFy8YjIrSBB1wSCYiIjIgW46eQZCaijhMSEZxOZv/s4KEkTdYBBMRETkQJWHn46HJzwMAQGXiF0Rdgk3BARG9jsx7r6uWEGCchWDYCIiSojJVPY1/dTpWHXR66gur4Eiwm/pilBQXV6DFyatwD/2fhS3AtwVK0hQrmJiHBERdatx13os2ljHZCqbqzx8DE4dWAF/0A9fwIdiTzG8bi/2tOxJuYIE21NTLuFKMBERxVm2eSkmv1zFZCoH8bq9GFg4MBrIRipIaKEIBcWeYjOHR2Q7DIKJiChG4671mLd2DpOpHM7r9qJq6ISke4Jdwo2a8olcBaacwyCYLCEBfCsEdigC3woBmekBEVGPFm2sgyLYjjcbTB89A6pMXEFClSFMGzXDohER2QeDYDLVAQE86s1DRWkvDO9fhFP7FWF4/yJUlPbCo948HBCZHiERdeYP+rFm2yomU2WJyrIxmD92QcIKEvPHLuAeb8pJDILJNG/muTCqXxHu7JWP7UpstLtdEbizVz5G9SvCm3mJV5yIyDp62/GSfdWOnIoVUxq6rSCxYkoDakdOzfAIiTKD1SHIFG/muXBlby8kACnil3sjt7VKiSt7e/H0AT/GtSf+yI6IzJdqO14mUzlDRVklKsoq4ypIEOUyrgST4Q4I4NqOAFjtJgDuTO3YH3xtby+3RhDZAJOpslvXChJEuYxBMBnuuYI8+JE8AI5QhYAfwPMFeaaOi4i0YTIVEeUCBsFkKAlgqdej67FLvB5WjSCyASZTEVEuYBBMhtorBLa5lG73ASciOx63j1siiGyByVRElO0clxi3evVqrFixAh999BGamppw1FFH4aqrrsKFF14I0SnweuGFF7B06VJ8+eWXKC8vx6xZs3DuuedmcOS54WCaQWyzECiVXA8msoNIMtW+1r3YdXA3ynoNQt+C0kwPCwCY4EVEaXNcELx8+XIcccQRmDdvHvr27Yv33nsPd955J3bv3o0ZM8L701atWoU777wT06dPR2VlJerr6zFjxgw8/fTTGD16dGafQJbrlWb8WsQAmMg2Gnetx6KNdVizbRVUqUIRCqqGTsCNo2dmbCuEHcdERM4kpHRW1LF3716UlsauRNx5552or6/HX/7yFyiKgvHjx2PkyJG4//77o8dcdtllKC4uxpIlS9K6fiikYu/eg2mdww7cbgV9+/bCvn0HEQxqqwmqhQRQUdoL2xWR0pYIISWOUiU27D2IbNwRYdZ8U8845+lZtnkp5q2dA0W4YhpnuIQbqgxh/tgFMVsirJjvVMeU7fg9bi3Ot7XSme/S0l5wuZLv+HXcnuCuATAADB8+HM3NzWhpacHOnTuxbds2VFdXxxxTU1OD9evXIxAIWDXUnCQAXOfXN8fX+wNZGQATOU3jrvWYt3YOJGRc57iQDEJCYu7a2diwqzGnx0REzua4ILg7H3zwAQ477DAUFRVhy5YtAIDy8vKYY44++mi0t7dj586dmRhiTrm0tR1eAIrGDxkUKeEFcElru6njIiJtFm2sgyISd3JUhAuLP6yzaET2HBMROZvj9gR39f7776O+vh5z584FABw4cAAAUFJSEnNc5OvI/elwu53/t0PkYwItHxekqh+AJ5rbcFlRPhQpE9YLVqSEAPBkcxv6mTAWuzBzvql7nHN9/EF/dL9tIiEZRP3WlWhHG7xur6nzrXdM2Y7f49bifFvLivl2dBC8e/duzJo1CxUVFbj66qstuaaiCPTt28uSa1mhpMScN4qLAKwCcCGAlo7bOq8LR8JirxB4CcAFxQWmjMNuzJpv6hnnPDWB5mZNLZMBQJUqXN4Q+hYd+p1oxnynO6Zsx+9xa3G+rWXmfDs2CG5qasL111+PPn364MEHH4SihP9S6N27NwDA5/NhwIABMcd3vl8vVZVoampJfqDNuVwKSkq8aGryIxQyZ4P/aQA2C+D3Hjcezc/D1k5/zQ0NqbihrR2XtwVRAmCfKSOwDyvmm2JxzvUJBV1QhKIp6FSEgpDfhX3tB02db71jynb8HrcW59ta6cx3SYlX0wqyI4Pg1tZWTJs2DT6fD8899xyKi4uj9w0bNgwAsGXLluj/j3ydl5eHwYMHp339bMoKDYVUU59PLwBT2wO49mAA+0S4DnCRlOgrD60GBxOdIMuYPd8Uj3Oemjzko2roBDRsWx2XgNaZS7hRXV6DPOTHzK8Z853umLIdv8etxfm2lpnz7biNLcFgELfccgu2bNmCpUuX4rDDDou5f/DgwRg6dCjWrFkTc3t9fT3GjBkDj0dfS19KjwBQKoEhqkRppwCYiOxn+ugZUGUo4TGqDGHaqBkWjcieYyIiZ3NcEPyzn/0Mb731FqZPn47m5mZs3Lgx+l+k/NnMmTOxcuVKPPDAA9iwYQPuuusubNq0CTfddFOGR09EZH+VZWMwf+wCCAi4ROwHhi7hhoDA/LELLG1OYccxEZGzOW47xLp16wAAv/71r+Pue+ONN3DkkUdi4sSJ8Pv9WLJkCR599FGUl5ejrq4OJ598stXDJSJypNqRUzG83wgs/rAO9VtXRruzVZfXYNqoGRkJNu04JiJKzM4tzh3XMS7T2DGO9OB8W49zbhwtb2JWz7ed31itwu9xa3G+U5Nui3N2jCMioozzur0YWDjQVsGmHcdERGHLNi/F5Jer0LBtdbSqiypVNGxbjUkvj8fyzY9leIRhDIKJiCgrSQDfCoEdisC3QoAfexKZz0ktzhkEExFRVjkggEe9eago7YXh/Ytwar8iDO9fhIrSXnjUm4cDLE9DZBontThnEExERFnjzTwXRvUrwp298rFdiY12tysCd/bKx6h+RXgzL/GbNBGlLtLiPFE9b+BQi3N/0G/RyLrHIJhyV1ACLTL8LxH1yB/0Y0/Lnoy/YSXzZp4LV/b2ohWAFAJSxAbBkdtaAVzZ25uTgbBTXktyJl/Al1KLc1/AZ/KIEnNciTSitO2SUDaFILYBQgJSAHIooJ7kAsr4OSlRRLrZ3VY6IIBre3shAagi8c+xKgQUKXFtby8+/LYZvXPg72AnvZbkXMWe4pRanBd7ipMeZyauBFNOER+pcL16KAAGwv+KbQjf/hHL3hABzsnujniuIA9+JA+AI1Qh4AfwfEGeqeOyA6e9luRcXrcXVUMnxDW06col3Kgpn5jx6i4Mgil37JJQ3lUhcCgAjhAdrZyVd1VgVw4sCxEl4KTsbiBcBWKp16PrsUu8nqyuGuG015Kcz0ktzhkEU85QNoXCkW4iouM4ohzmpOxuANgrBLa5lLg9wMnIjsfty+JdUE57Lcn5nNTinEEw5YagjNkC0ZPI1ggmy1Guclp2NwAcTDOIbU4xeHYKJ76WlB1qR07FiikNqC6vgSLCoWakxfmKKQ2oHTk1wyMMY2Ic5YZA8gA4Qsjw8fzpoFykJ7s70/v6eqX5N2uRzM4/ep34WtqZP+hHoLkZoaALecjP9HBsr6KsEhVllbZucc63ecoNnnAVCC2BsBTh44lykdOyuwGgVEoMDanYrsSXRUtESImjVIm+2RkDO/K1tCNW1kiP1+21XfAbwe0QlBvcAnJoR4CbQKRcGtzZ+fEoUTKR7G6R5O1BQLFFdnd4LMB1/oCux17vDyRNFXAqp2Xq2xEra2Q3BsGUM9STXEiaBi47jiPKYWOPPAcSiVcPJVScdeS5Fo0ouUtb2+EFoGjc2qBICS+AS1rbTR1XpjkpU99uWFkj+zEIptxRJqCepUAifkVYinB8rJ6lsGEG5by1n78dTWbpiSIUvPv5WxaNKLneEnj8gD9c6jBJIKxICQFg2QF/1jfKcFKmvt2wskb2YxCcq4ISarOac1UQ5AgFocmumK0RkS0QockuyBH8kaDcFqkokGwfqSpV21UUGNcewtMH/ChAeL+v6BIMR24rAPDMAT/ObbdnOUSjWxs7JVPfTlhZIzcwMS7XdLQMxjbggDwQros7NMdaBpcJqGXu8B8AAYST4LgHmAiA8ysKjGsPYelHb+Hulk/xj2PGAaXHRO87LODHzHYFl7a2o8SGf/+bmYDlhEx9O3H6zwFpwyA4h4iP1HBHNIFDe2MjLYO3hsJbBXJpJdQt+BNA1IXTKwos27wU89bOCX+MLYOAtxTwFENp92N3y9fIG7sAJTZc+ew87q4JWKu3rsT8sQsMWbG1c6a+nTj954C0yaGIJ8exZTARaeDkigLdJjL59wIHtkNt2QPYNJGJCVj24+SfA9KOQXCOYMtgItLKqRUFnJrI5NRxZzun/hyQdgyCcwFbBhNRCpxYUcCpiUxOHXcucOLPAaWGQXAuaJGptwzOhKAEWqQ5QbiZ5ybKQk6rKKAnkSkdRlVwsHrclBqn/RxQapgWlM06KkGIrdofkpGWwZFxbgsH4ZGSZYZUrDDz3ERZzkkVBaxKZDK6ggMTsOwv8nPQjja4vCGE/C7kIT/TwyIDcCU4S4mPVLhe7Qj+ND4mEy2DY8bZsUgb2ZbhejUE8ZG2FRKrz02US7xuLwYWDrRtAAxYk8hkRgtdJmA5h9ftxWFFh/E1yCIMgrNRgkoQCVndMtjMihWshkGUc8xMZDKzggMTsIgyg0FwFtJUCaKTTLUMNrNiBathEOUeMxOZzKzgwAQsosxgEJxtNFaCiMpUy2AzK1awGgZRzjIjkcmKCg5MwCKyHhPjsk0gxS0Ql7uglmQgQSyFcUYrVmj9bjXz3ERkS52T9yKJTPta92LXwd0o6zUIfQtKdZ/bqha6TkpEJMoGfOvPNp7w9gbNgfCzIShDM1AtIYVxplyxwsxzE5GtdFet4bRBlRAQ2LB7vSMrOLC1MZE1uB0i27gF5NCO4E6LTFVL0DhOXRUrzDw3EdlGT9UaGne9h/W71rGCAxElxCA4C6knucKZbhplqlqCpnHqrFhh5rmJKPMSVWvoDis4EFFXDIKzUZmAepYCiRRWhAHrqyUkGGfaFSvMPDcRZZyWag3dYQUHIorgnuAsJUcoCJWKcFC7VVvFNCEBbEO4WoJFWwRixrnN2K5uZp6bKFs5ISkrUq1Ba7JaZ50rOKT6/GpHTsXwfiOw+MM61G9dGd1vXF1eg2mjZjAAJnIYBsHZrExALXMDTSrcz2h7s8hItYTIOIMyfG0PjAvCzTw3URYxuh2wmVKp1tAdVnAgIoDbIXJDodC8LSKj1RLcAigU5gSpZp6byOHMaAdspki1Br2MquBg91bSRJQYg+BcwGoJRNQDM9sBm0VrtYbusIIDEUUwCM4RrJZARN0xsx2wmbRUa+gOKzgQUQSD4FzBaglE1IUV7YDNkqhaQ3dYwYGIumIQnEPkCAWhya7wlodIrNuxBSI02QU5gt8ORHbkD/qxp2WP4UGonnbAdlI7cipWTGlAdXlNdI+wIhScfvgZOP3wM2Nuqy6vwYopDagdOTWTQyYiG2F1iFzTUS1BgUBvbyEO+FugptJZg4gsY3bFBqvbAZshUbUGVnAgokS49Jer3AJKkcIkOCKbsqJiQza1A+6uWgMrOBBRIgyCiYhsxsqKDWwHTES5ikEwEZHNWFmxge2AiShXMQgmIrKRTFRs6CnBjMlkRJTNmBhHRGQjeio2dLfnNdWkMKe3A3bquIkocxgEExHZSLoVG9KtKOF1ex0VRJpdQYOIshe3QxAR2Ug6FRusqChhJ7n2fInIWAyCiYhsRk/FBisrSthBrj1fIjIeg2AiIpvRU7HByooSdpBrz5eIjMcgmIjIhlKp2JCJihKZlGvPl4jMwcQ4IiKbilRs2Ne6F7sO7kZZr0HoW1Aad5xRFSWcIteeLxGZg0EwEZFNaa18kG5FCafJtedLRObgdggiIhtKpfJBOhUlnCjXni8RmYNBMBGRzeipfKCnooST5drzJSLjMQgmIrIZPZUP9FSUcLJce75EZDwGwURENpJO5YNUKkpkg1x7vrnOH/RjT8seVvsgwzAxjojIRtKtfBCpKOEP+uEL+FDsKc7qPbG59nxzEVtjk1m4EkxEZCORygdaJKp84HV7MbBwYM4EhLn2fHMFW2OTmRgEExHZCCsfEIWxNTaZjUEwEZHNsPIBEVtjk/kYBBMR2QwrH1CuY2tssgKDYCIiG0q18gEz5ymb6EkQJUoVq0MQEdmUlsoHzJynbMTW2GQFrgQTEdlcT5UPmDlP2YoJomQFBsFERA7EzHnKdkwQJbMxCCYiciBmzlO2Y4IomY1BMBGRwzBznnIFW2OTmZgY5yRBCQQAeAC4RfrHEZEjpdtamchJ2BqbzMIg2Al2SSibQhDbACEBKQA5FFBPcgFlIvXjiMjRmDlPucjr9jL4JUNxO4TNiY9UuF49FNgC4X/FNoRv/0hN6Tgicj5mzhMRpY9BsJ3tklDeVSFwKLCNEBIQAJR3VWCzqu24XV3uJCLHYuY8EVF6GATbmLIpFI5gExGA66+qpuOUTYnfMInIOZg5T0SUHgbBdhWUMVsbeiIkgBZtx4lt4fOaLiiBFmnNtYhymN1bK7OVMxHZGRPj7CqQPLCN0JryJmT4vKa96kzMI7KchIQqJaQM/8KQMvx1Z1a3VmYrZyJyAiGl5HJdCkIhFXv3HjT/QkEJ12MhTYGwhLZAWAogNNUFuAXcbgV9+/bCvn0HEQymnzQnPgrvS4aIDd6lCA9QPUuBHJG7HzwYPd+UXC7M+bLNSzFv7RwowhVTM9gl3FBlCPPHLoCETHqMEbVWI/N9/zu/wW1vzzb9epQb3+N2wvm2VjrzXVraCy5X8piDK8F25RaQQwFsS7wiLAUALyD9yY+TQ2FO3eBOCXzoJjEPCCfmhUoFV4SJDJKsbTIA3L52VtxtXb+eu3Y2hvcbYcgK7Z93/Bm3vT074ZiMvB4RUTpyd2nOAdSTXHFBZRwJhE5RNB2nnpS4xapeWhP4mJhHZBwtbZMBQCT54TSytfKC9QvgYitnInIIBsF2VibC2wjQseLbiRThuFc9SwFGKtqOM2MVNoUEPssS84iynNa2yUB4z3AiRrVW9gf9ePVfryLIVs5E5BDcDmFzcoSCUKkIr6Ju6znhTOtxhkslgc+IxDy9LaHZSpqySCptk7UworWyL9DEVs5E5CgMgp2gTEAtcycP5LQeZyRPONjWlMAnOsakh97KE6xYQVkolbbJWhjRWrnYU8JWzkTkKNwO4SRuARSK5IGt1uMMGpMcGr8No6t0EvP0toRmK2nKVlrbJgPJ9wQb1VrZ6/Zi8nGT4WYrZyJyCAbBlDatCXy6EvO0to7u2hJa7+OIHEJL22Qg+Z5gI1srzx4zGyG2ciYih2AQTOnTmsCnY/uB3soTrFhB2U5L2+R7xy7EvWMXWtZa+cwhZ+K+c627HhFROrgnmAxhSmJeKq2jt4WPh1vofxyRw9SOnIrh/UZg8Yd1qN+6Mtqdrbq8BtNGzYgGm1qOMco1J16HY/ucYNn1iIj0YhCcC6xKlDM6MU9v5QmrK1YQZVBFWSUqyirhD/rhC/hQ7CmO22+r5Rirx0RElGl8689miSojDDbxum5hzHeW3soTVlWsILIRr9ubNNDUcoyRrL4eEVEquCc4SyWrjIDNDtgLq7fyhAUVK4iIiMjZGARnIw2VEfB2CMGdybtNZZreyhOmVqwgIiIix2MQnIW0VkZobWy1ZDxp0Vt5wsSKFUREROR83BOcbTRWRoAE2v/VDpyTZ8Wo0qK38kTGWkkTERGR7TEIzjYpVEZApDKCExLD9FaeyEQraSIiIrI9BsHZJoXKCHBiZQS9lSeMqlhBREREWYF7grONxsoIEEDecXlcFSUiIqKcxCA4C2mtjFBQWWDJeIiIiIjshkFwNtJQGQHnuOAezP0BRERElJsYBWWpZJUR3INZH5eItNnXuhe7Du5GWa9B6FtQmunhEBEZgkFwNmNlBCJKw+N/X4KFH/wvvmrZHb3tsMJBmH3qXFwzcmoGR0ZElD4GwbmAlRGIKEU3vH4NXvn0xbjbv2rZjblrZ6Hxy3VYfMHjGRgZEZExuCeYiIhiPP73Jd0GwJ29/OkfsGzzYxaNiIjIeAyCiYgoxsIP/lfbce/fa/JIiIjMwyCYiIii9rXujdkDnMjull3Y17rX5BEREZkjq4Pgzz77DNdccw1Gjx6NM844A/feey8CgUCmh0VEZFu7DmoLgPUeT0RkF1mbLnXgwAH84Ac/wNChQ/Hggw/iq6++wq9//Wu0trbiJz/5SaaHR0RkS2W9Bpl6PBGRXWRtEPz73/8eBw8eRF1dHfr06QMACIVC+NnPfoZp06bhsMMOy+wAiYhsqG9BKQ4rHKRpS8SgwjLWDSYix8ra7RBr167FmDFjogEwAFRXV0NVVaxbty5zAyMisrlZ37lN23Gn3m7ySIiIzJO1QfCWLVswbNiwmNtKSkowYMAAbNmyJUOjIiKyv2tPvB5Tjrko4TFTjrmIDTOIyNGydjtEU1MTSkpK4m7v3bs3Dhw4kNa53W7n/+3gcikx/5K5ON/W45yn57Ga5Th90xm4/y/3YvfBXdHbB/Uqw5z/uB1TT7o+5njOt/U459bifFvLivnO2iDYLIoi0Ldvr0wPwzAlJd5MDyGncL6txznX79azb8GtZ9+CvS178aXvSxxefDhKCxPvAeZ8W49zbi3Ot7XMnO+sDYJLSkrg8/nibj9w4AB69+6t+7yqKtHU1JLO0GzB5VJQUuJFU5MfoZCa6eFkPc639TjnxhHIxxGecqAN2Nd2sNtjON/W45xbi/NtrXTmu6TEq2kFOWuD4GHDhsXt/fX5fPj666/j9gqnKhjMnm/+UEjNqudjd5xv63HOrcX5th7n3Fqcb2uZOd9Zu7Fl7NixeO+999DU1BS9bc2aNVAUBWeccUYGR0ZEREREmZa1QfBll12GXr164eabb8af//xnvPjii7j33ntx2WWXsUYwERERUY7L2iC4d+/e+P/t3XtUlGUeB/DvQMKmCEQX3VB2kc47gYyiIiOBWGTcYpXMFi3FE3gtUPHs4eKqpZKX3UzLC51Q85paXra4VJqYliFHy6Sym4AKdNbtADIDKAHz7B8u7/o6Q86hAWLe7+ccT83zPDPzmx/vmfnyzsPM9u3b4ejoiOeffx5r1qzBxIkTkZGR0d2lEREREVE3s9s9wQDg4+ODbdu2dXcZRERERPQ7Y7dngomIiIiI2sMQTERERESqwxBMRERERKrDEExEREREqsMQTERERESqwxBMRERERKrDEExEREREqsMQTERERESqwxBMRERERKrDEExEREREqsMQTERERESqwxBMRERERKrDEExEREREqsMQTERERESqoxFCiO4uoicRQsBkso+WOTo6oLXV1N1lqAb73fXY867Ffnc99rxrsd9dq6P9dnDQQKPR3HYdQzARERERqQ63QxARERGR6jAEExEREZHqMAQTERERkeowBBMRERGR6jAEExEREZHqMAQTERERkeowBBMRERGR6jAEExEREZHqMAQTERERkeowBBMRERGR6jAEExEREZHqMAQTERERkeowBBMRERGR6jAE26n3338fc+bMQVhYGAICAjB+/Hjs378fQgjFunfeeQeRkZHQ6XQYN24cjh071k0V25eGhgaEhYVBq9Xiq6++Usyx57Z16NAhxMXFQafTQa/XY/r06bh+/bo8X1hYiHHjxkGn0yEyMhIHDhzoxmp7tqNHj+Kpp57CsGHDEBoainnz5qGiosJsHY/xjrl06RKWLFmC8ePHw8/PD7GxsRbXWdNfo9GIhQsXIigoCMOGDcPcuXPxn//8p7MfQo9yu37X19dj/fr1mDhxIgIDA/HQQw9h9uzZ+P77781ui/22jrXHeJuPPvoIWq3W4jpb9Jwh2E5t27YNd955JzIyMpCdnY2wsDAsXrwYGzdulNfk5+dj8eLFiI6ORk5ODgICApCcnIwvv/yy+wq3E5s2bUJra6vZOHtuW9nZ2Vi+fDliYmKwZcsWLFu2DAMGDJB7f+bMGSQnJyMgIAA5OTmIjo7G3//+d3zwwQfdXHnPU1xcjOTkZDzwwAPYuHEjFi5ciO+++w6JiYmKXzp4jHfcjz/+iOPHj+NPf/oTfHx8LK6xtr/z58/HyZMn8eKLL+Lll19GeXk5ZsyYgZaWli54JD3D7fr9008/Yd++fQgJCcG6deuwfPlyGI1GxMfHo7S0VLGW/baONcd4m+vXr2PFihW45557LM7bpOeC7FJ1dbXZ2KJFi8Tw4cNFa2urEEKIiIgIsWDBAsWa+Ph4MX369C6p0V5duHBBBAQEiD179ghJkkRJSYk8x57bTmlpqfDz8xMff/xxu2sSExNFfHy8YmzBggUiOjq6s8uzO4sXLxbh4eHCZDLJY0VFRUKSJHH69Gl5jMd4x7U9NwshRHp6unj88cfN1ljT3y+++EJIkiQ++eQTeay0tFRotVqRn5/fCZX3TLfrd0NDg2hsbFSM1dfXi6CgILFs2TJ5jP22njXHeJt169aJZ555xuI6W/WcZ4LtlIeHh9mYr68v6uvr0djYiIqKCly8eBHR0dGKNTExMSgqKsIvv/zSVaXanaysLEyaNAne3t6Kcfbctg4ePIgBAwZgzJgxFud/+eUXFBcXIyoqSjEeExOD0tJSVFZWdkWZdqOlpQV9+vSBRqORx/r27QsA8jYrHuO/jYPDr78kW9vfEydOwNXVFSEhIfKaQYMGwdfXFydOnLB94T3U7frdu3dv3HnnnYqxPn36wMvLS/G2O/ttvdv1vM3ly5fx5ptvYtGiRRbnbdVzhmAV+fzzz9GvXz+4uLigrKwMAMyCmo+PD5qbmy3u86Pb++CDD/DDDz/g+eefN5tjz23r3LlzkCQJmzZtQnBwMPz9/TFp0iScO3cOwI0n0ebmZgwaNEhxvba34Np+HmSdCRMmoLS0FLt374bRaERFRQVeeeUV+Pn5Yfjw4QB4jHc2a/tbVlYGb29vxS8swI2QwOP+tzEYDPjxxx8Vzyvst+299NJLGD9+PB588EGL87bqOUOwSpw5cwYFBQVITEwEANTV1QEAXF1dFevaLrfNk/WuXbuGVatWITU1FS4uLmbz7Llt/fzzz/j000/x7rvv4oUXXsDGjRuh0WiQmJiI6upq9tvGAgMDsWHDBqxZswaBgYEYO3YsqqurkZOTA0dHRwA8xjubtf01GAzyWfqbubm58WfwG/3zn/+ERqPB5MmT5TH227YKCwtx9uxZzJs3r901tuo5Q7AK/Pvf/0Zqair0ej0SEhK6uxy7lZ2djbvvvhtPPvlkd5eiCkIINDY24tVXX0VUVBTGjBmD7OxsCCGwa9eu7i7P7nzxxRdIS0vDX//6V2zfvh2vvvoqTCYTZs6cqfjDOCJ7deDAAbz99ttYsmQJ+vfv393l2KWmpiasWLECKSkpFrd12hpDsJ0zGAyYMWMG3N3dsX79enk/jpubG4AbHzFy6/qb58k6VVVV2Lp1K+bOnQuj0QiDwYDGxkYAQGNjIxoaGthzG3N1dYW7u7vi7TJ3d3f4+fnhwoUL7LeNZWVlYdSoUcjIyMCoUaMQFRWFN954A+fPn8e7774LgM8rnc3a/rq6uqK+vt7s+nV1dfwZdNDx48exZMkSPPfcc3jiiScUc+y37Wzfvh0ODg54/PHHYTAYYDAY0NzcDJPJBIPBIO97t1XPGYLt2PXr1zFr1iwYjUZs3rxZ8dZB236mW/fOlJWVoVevXhg4cGCX1trTVVZWorm5GTNnzsTIkSMxcuRIzJ49GwCQkJCAZ599lj23sQceeKDduaamJnh5eaFXr14W+w3AbK8w/brS0lKz/Xn9+/fHXXfdhcuXLwPg80pns7a/gwYNQnl5udnnwpeXl/O474Avv/wS8+bNQ1xcnMW36Nlv2ykrK8OlS5cQHBwsv5bm5eWhtLQUI0eOlD/n3VY9Zwi2Uy0tLZg/fz7KysqwefNm9OvXTzE/cOBA/PnPfzb7vNSCggIEBwfDycmpK8vt8Xx9fbFjxw7Fv8zMTADA0qVL8cILL7DnNvbII4/g6tWr+Pbbb+Wx2tpafPPNNxg8eDCcnJyg1+vx4YcfKq5XUFAAHx8fDBgwoKtL7tHuv/9+nD9/XjFWVVWF2tpaeHp6AuDzSmeztr9hYWGoq6tDUVGRvKa8vBznz59HWFhYl9bc0124cAGzZs3CqFGjsHTpUotr2G/bmTFjhtlraWhoKDw9PbFjxw6Eh4cDsF3P77D5I6DfhaVLl+LYsWPIyMhAfX294oPU/fz84OTkhJSUFPztb3+Dl5cX9Ho9CgoKUFJSwv2UHeDq6gq9Xm9xbvDgwRg8eDAAsOc2NHbsWOh0OsydOxepqalwdnbGG2+8AScnJzz99NMAgDlz5iAhIQEvvvgioqOjUVxcjLy8PKxdu7abq+95Jk2ahBUrViArKwvh4eG4evWqvA/+5o/s4jHecdeuXcPx48cB3PgFo76+Xg68QUFB8PDwsKq/bd/ot3DhQqSnp8PZ2Rlr166FVqtFREREtzy236Pb9VsIgaSkJDg7O2PatGn4+uuv5eu6uLjI70ax39a7Xc99fHzMvkTj0KFDuHLliuI11lY914hbzyWTXQgPD0dVVZXFuaNHj8pnwd555x3k5OTgp59+gre3NxYsWIBHHnmkK0u1W8XFxUhISMD+/fuh0+nkcfbcdmpqarBy5UocO3YMzc3NCAwMRGZmpmKrxNGjR7Fu3TqUl5fj/vvvx8yZMzFx4sRurLpnEkJg79692LNnDyoqKtCnTx8EBAQgNTXV7EWLx3jHVFZW4tFHH7U4t2PHDjkEWNNfo9GIlStX4siRI2hpaUFoaCgWLVpk9q6gmt2u3wDa/WPyoKAg7Ny5U77MflvH2mP8ZhkZGfj666+Rl5enGLdFzxmCiYiIiEh1uCeYiIiIiFSHIZiIiIiIVIchmIiIiIhUhyGYiIiIiFSHIZiIiIiIVIchmIiIiIhUhyGYiIiIiFSHIZiIyM4dPHgQWq0WlZWV3V0KEdHvBkMwEVE7du/eDa1Wi6eeeqq7S+kS69evh1arRU1NTXeXQkTU6RiCiYjakZubC09PT5SUlODSpUvdXQ4REdkQQzARkQUVFRU4e/YsMjMz4eHhgdzc3O4uiYiIbIghmIjIgtzcXLi5uWHMmDGIjIy0GIIrKyuh1WqxZcsW7Nu3D2PHjoW/vz+efPJJlJSUKNZmZGRg2LBhuHLlCp577jkMGzYMo0aNwurVq9Ha2iqvKy4uhlarRXFxscX7OnjwoDz23XffISMjA48++ih0Oh1CQkKQmZmJ2tpam/Vh6tSpiI2NxYULFzB16lQMHToUo0ePRk5OjtnapqYmrF+/HpGRkdDpdAgNDUVycjIuX74sr2lsbMSqVaswZswY+Pv7IzIyElu2bIEQQnFbWq0Wy5Ytw/vvv4+YmBgMGTIE8fHx+P777wEAe/fuxWOPPQadToepU6da3O987tw5JCUlYcSIERg6dCimTJmCzz//3Ga9IaKejSGYiMiC3NxcPPbYY3ByckJsbCwuXrxoFmzb5OXlYcuWLYiPj8f8+fNRVVWFlJQUNDc3K9a1trYiKSkJ7u7uSEtLQ1BQELZu3Yp9+/Z1qMbPPvsMFRUVmDBhAhYvXoyYmBgUFBRg5syZZqHyt6irq8P06dPx4IMPIj09HYMGDcLLL7+M48ePy2taW1sxa9YsbNiwAYMHD0ZGRgYSEhJgNBrxww8/AACEEJgzZw62bduG0aNHIzMzE97e3vjHP/6BlStXmt3vmTNnsHr1asTFxSE5ORmlpaWYPXs2du/ejZ07d+Lpp59GUlISzp49i4ULFyquW1RUhGeeeQYNDQ1ITk5GamoqDAYDpk2b1u7PkYhURhARkcJXX30lJEkSJ0+eFEIIYTKZRFhYmMjKylKsq6ioEJIkiaCgIHH16lV5/KOPPhKSJInCwkJ5LD09XUiSJDZs2KC4jbi4OPHEE0/Il0+dOiUkSRKnTp2yeF8HDhyQx65du2ZWe15enpAkSZw+fVoeO3DggJAkSVRUVPzq437ttdeEJEmiurpaHpsyZYqQJEkcOnRIHmtqahIhISEiJSVFHtu/f7+QJEm8+eabZrdrMpmEEEIcOXJESJIkNm3apJhPSUkRWq1WXLp0SR6TJEn4+/srat67d6+QJEmEhIQIo9Eoj69Zs0bx+Ewmk4iIiBCJiYnyfQtxo1/h4eHi2Wef/dU+EJE68EwwEdEtcnNzcc8990Cv1wMANBqNfJb15q0LbWJiYuDm5iZfDgwMBHBjX/GtJk+erLg8YsSIDn902R/+8Af5/5uamlBTU4OhQ4cCAL755psO3aYlvXv3xvjx4+XLTk5O0Ol0isd3+PBh3HXXXZgyZYrZ9TUaDQDgxIkTcHR0xNSpUxXziYmJEELgxIkTivHg4GAMGDBAvtz22CIiIuDi4iKPDxkyBMD/+/3tt9/i4sWL+Mtf/oLa2lrU1NSgpqYGjY2NCA4OxunTp2EymTrUCyKyH3d0dwFERL8nra2tyM/Ph16vV4TTIUOGYOvWrSgqKkJoaKjiOn/84x8Vl9sCscFgUIw7OzvDw8PDbG1dXV2Har169So2bNiAgoICVFdXK+aMRmOHbtOS/v37y0G2jZubm7w/FwAuX74Mb29v3HFH+y8rVVVVuO+++xQBFgB8fHzk+Zvd2te26/Xv318x3rdvXwD/7/fFixcBAOnp6e3WYjQaFb+4EJH6MAQTEd3k1KlT+Pnnn5Gfn4/8/Hyz+dzcXLMQ7OjoaPG2xC37cttbd7Nbw2YbS2cu58+fj7NnzyIpKQm+vr7o3bs3TCYTpk+fbtM9wdbU3Rnau9/b9bvtv2lpafD19bW4tnfv3jaokIh6MoZgIqKb5Obm4u6778aSJUvM5o4cOYIjR45g6dKliq0ItuTq6grA/EzurWdJ6+rqUFRUhJSUFCQnJ8vjbWdBu5qXlxfOnTuH5uZm9OrVy+IaT09PFBUVob6+XnE2uKysTJ63hYEDBwK4ceb4oYcessltEpH94Z5gIqL/uX79Og4fPoyHH34YUVFRZv/aPm2gsLCw02rw9PSEo6MjTp8+rRjfs2eP4nJ7Z0O3b9/eabX9moiICNTW1mL37t1mc21nZsPCwtDa2mq2Ztu2bdBoNAgLC7NJLf7+/vDy8sLWrVvR0NBgNs9vxCMigGeCiYhkhYWFaGhoQHh4uMX5gIAAeHh44L333kNMTEyn1NC3b19ERUVh165d0Gg0GDhwID7++GOzPb8uLi4YOXIkNm/ejObmZvTr1w8nT57s8B/Z/VZxcXH417/+hZUrV6KkpAQjRozAtWvXUFRUhMmTJ2Ps2LEIDw+HXq/H2rVrUVVVBa1Wi5MnT+Lo0aOYNm0avLy8bFKLg4MDsrKyMGPGDMTGxmLChAno168frly5guLiYri4uOD111+3yX0RUc/FEExE9D/vvfcenJ2dERISYnHewcEBDz/8MHJzc236hRS3WrRoEVpaWrB37144OTkhKioKaWlpiI2NVaxbs2YNli9fjrfeegtCCISEhCAnJwejR4/utNra4+joiJycHGRnZyMvLw+HDx+Gu7s7hg8fDq1WC+BG/7Kzs/Haa6+hoKAABw8ehKenJ9LS0pCYmGjTevR6Pfbt24dNmzZh165daGxsxL333it/6QYRkUbY8q8niIiIiIh6AO4JJiIiIiLVYQgmIiIiItVhCCYiIiIi1WEIJiIiIiLVYQgmIiIiItVhCCYiIiIi1WEIJiIiIiLVYQgmIiIiItVhCCYiIiIi1WEIJiIiIiLVYQgmIiIiItVhCCYiIiIi1WEIJiIiIiLV+S8leLOltZ7KkwAAAABJRU5ErkJggg==\n"
          },
          "metadata": {}
        }
      ]
    }
  ]
}
