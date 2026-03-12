# Single-Step IR Thermal Data Processing Workflow

```python
# locate heater region on the thermal frame
heater_roi = detect_heater(frame0)

temperature = []

for i in range(n_frames):
    frame = read_ir_frame(header, i)
    heater = frame[heater_roi]

    temperature.append(heater.mean())

# convert camera signal to temperature using calibration
avg_val = calibrate_temperature(temperature, calib_param)

# save aggregated results for each heat-load step
wb = load_workbook(filename) # load the existing experimental journal with pre-written heat-flux data
sheet = wb['Heat transfer curve']
for col in heat_flux_columns:
        n_row_in_col = len([x for x in df[df.columns[col-1]][2:] if not np.isnan(x)]) 
        for row in range(n_row_in_col):
            sheet.cell(column = col + 1, row = row + 4).value = avg_val # writing into corresponding cells
wb.save(filename = output_file)
