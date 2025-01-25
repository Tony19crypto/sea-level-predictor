# sea-level-predictor
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

def draw_plot():
    # Import data
    df = pd.read_csv('epa-sea-level.csv')

    # Create scatter plot
    plt.figure(figsize=(10, 6))
    plt.scatter(df['Year'], df['CSIRO Adjusted Sea Level'], label='Data', color='blue', alpha=0.6)

    # Create first line of best fit using all data
    slope_all, intercept_all, _, _, _ = linregress(df['Year'], df['CSIRO Adjusted Sea Level'])
    years_extended_all = pd.Series(range(1880, 2051))
    plt.plot(years_extended_all, slope_all * years_extended_all + intercept_all, color='red', label='Best Fit: 1880-2050')

    # Create second line of best fit using data from 2000
    df_recent = df[df['Year'] >= 2000]
    slope_recent, intercept_recent, _, _, _ = linregress(df_recent['Year'], df_recent['CSIRO Adjusted Sea Level'])
    years_extended_recent = pd.Series(range(2000, 2051))
    plt.plot(years_extended_recent, slope_recent * years_extended_recent + intercept_recent, color='green', label='Best Fit: 2000-2050')

    # Add labels and title
    plt.xlabel('Year')
    plt.ylabel('Sea Level (inches)')
    plt.title('Rise in Sea Level')
    plt.legend()
    plt.grid(True, linestyle='--', alpha=0.5)

    # Save plot and return data for testing (if needed)
    plt.savefig('sea_level_plot.png')
    return plt.gca()

if __name__ == "__main__":
    draw_plot()
