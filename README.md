# Ann Arbor Parking Ticket Analysis
Project Conducted in SIADS 505-University of Michigan School of Information

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Functionality](#functionality)
  - [Loading Data](#loading-data)
  - [Generating Descriptive Statistics](#generating-descriptive-statistics)
  - [Common Car Make Analysis](#common-car-make-analysis)
  - [License Plate Analysis](#license-plate-analysis)
- [Setup Instructions](#setup-instructions)
- [Key Results](#key-results)
- [Contact Information](#contact-information)

## Project Overview
This project analyzes parking ticket data from the city of Ann Arbor obtained through FOIA requests. The data consists of multiple Excel files, each containing information on ticket violations. The primary goal of this project is to load, clean, and analyze this data to gain insights into parking violations, car makes, and license plate distributions.

## Data Sources
The data consists of parking ticket records for Ann Arbor from the following Excel files:
- AnnArbor-TicketViolation2015.xls
- AnnArbor-TicketViolation2016.xls
- AnnArbor-TicketViolation2017.xls
- AnnArbor-TicketViolation2018.xls
- AnnArbor-TicketViolation2019.xls
- AnnArbor-TicketViolation-jan2020.xls

## Functionality

### Loading Data
The `load_ticket_data()` function reads and processes multiple sheets from each Excel file, cleans the data by removing headers and footers, and combines them into a single DataFrame.

```python
def load_ticket_data():
    def process_file(file):
        data = pd.read_excel(file, sheet_name=None, header=None)
        frames = []
        
        for sheet_name, df in data.items():
            if not df.empty:
                df = df.iloc[4:]  # Adjusting for header rows
                df.columns = ['Ticket #', 'Badge', 'Issue Date', 'IssueTime', 'Plate', 'State', 
                              'Make', 'Model', 'Violation', 'Description', 'Location', 'Meter', 
                              'Fine', 'Penalty']
                
                # Remove footer row in the third sheet if present
                if sheet_name == 'Sheet3':
                    df = df[:-1]
                    
                frames.append(df)
        return pd.concat(frames, ignore_index=True) if frames else pd.DataFrame()
```

### Generating Descriptive Statistics
The `generate_descriptors(df)` function analyzes ticket descriptions to understand their distribution across different times of the day:
- Morning (3 AM - 11:59 AM)
- Afternoon (12 PM - 5:59 PM)
- Evening (6 PM - 2:59 AM)

### Common Car Make Analysis
The `common_car_make(df)` function identifies the most common make of car that received tickets from the state of NY.

### License Plate Analysis
The `fine_per_plates(df)` function analyzes the distribution of different types of Michigan license plates that received tickets:
- Standard format (ABC1234)
- Legacy formats (ABC123, 123ABC)
- Vanity plates

## Setup Instructions

1. Clone the Repository:
```bash
git clone https://github.com/AshJaffer/AnnArbor_ParkingTicket_Analysis.git
cd AnnArbor_ParkingTicket_Analysis
```

2. Install Dependencies:
```bash
pip install pandas xlrd jupyter
```

3. Place Data Files:
- Create a `data` folder in the repository
- Add the Excel files to the `data` folder
- Update file paths in the `load_ticket_data()` function if necessary

4. Run the Jupyter Notebook:
```bash
jupyter notebook parking_analysis.ipynb
```

## Key Results
- Successfully loaded and processed 811,439 parking ticket records
- Identified the most common car make for NY-registered vehicles
- Analyzed the distribution of Michigan license plate formats
- Generated comprehensive statistics on violation types by time of day

## Contact Information
