> ### 2. TXT 格式

> ### 3. JSON 格式

> ### 4. CSV 格式


import xlsxwriter
 
 
workbook = xlsxwriter.Workbook('G:\\xxoo\\103.xlsx')
        #在G盘xxoo文件下创建103的excel
worksheet = workbook.add_worksheet('s001')
		#103的excel的sheet页名称为s001
worksheet.write(0,0,123456)
worksheet.write(2,1,664)
worksheet.write(1,5,250)
		#写入信息
workbook.close()