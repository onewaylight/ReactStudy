```js
import type { NextPage } from "next";
import React, { useState } from "react";
import axios from "axios";
const Monitoring1: NextPage = () => {
  const [pValue, setpValue] = useState(0);
  const [file, setFile] = useState();
  const [uploadedFile, setUploadedFile] = useState({});

  const onChange = (e: React.FormEvent<HTMLInputElement>) => {
    setFile(e.target.files[0]);
    console.log(e.target.files[0].name);
  };

  const onSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log(file);
    const formData = new FormData();
    formData.append("file", file);
    formData.append("data", { name: "gofogo" });

    try {
      const res = await axios.post("/api/upload", formData, {
        headers: {
          "Content-Type": "multipart/form-data",
        },
        onUploadProgress: (progressEvent: any) => {
          let pert = (progressEvent.loaded * 100) / progressEvent.total;
          console.log(pert);
          setpValue(pert / 100);
        },
      });
      console.log(res.data);
      const { ok, result } = res.data;

      setUploadedFile({ ok, result });
    } catch (err) {}
  };

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input type="file" onChange={onChange} />
        <input type="submit" value="upload" />
      </form>
      <progress value={pValue} />
    </div>
  );
};

export default Monitoring1;
```


```js
import axios from 'axios'
import React, {useState} from 'react'


const UploadForm: React.FC = () => {
	const [file, setFile] = useState<FileList | undefined>();
	const [fileName, setFileName] = useState<string>('이미지 파일을 업로드 해주세요');

	const imageSelectHandler = (e: React.ChangeEvent<HTMLInputElement>) => {
		const fileList = e.target.files;
		if (fileList != null) {
			setFile(fileList);
			setFileName(fileList[0].name);
		}
	}
	
	const onSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
		e.preventDefault();
		const formData = new FormData();
		formData.append("image", file );
	
		try {
			const res = await axios.post('/server/upload/', formData, {
				headers: {"Content-Type": "multipart/form-data"}
			})
			console.log({res});
			alert(`success !!`);
			
		} catch (e) {
			alert(`fail!!`);
			console.log(e);
		}
	}


	return (
		<form onSubmit={onSubmit}>
			<label htmlFor='image'>{fileName}</label>
			<input 
				id='image'
				type='file'
				multiple
				onChange={imageSelectHandler}
			/>
			<button type='submit'>제출</button>
		</form>
	)
}

export default UploadForm
```

```js
import React from 'react';
import { Formik, Form, Field } from 'formik';
import * as yup from 'yup';
import AdapterDateFns from '@mui/lab/AdapterDateFns';
import LocalizationProvider from '@mui/lab/LocalizationProvider';
import { DesktopDatePicker } from 'formik-mui-lab';
import Button from '@mui/material/Button';


const DisplayingErrorMessagesSchema =  yup.object().shape({
  startDate: yup.date().nullable()
    .typeError('Start date is required')
    .required('Start Date is required'),
  endDate: yup.date().nullable()
    .when("startDate",
    (startDate, yup) => startDate && yup.min(startDate, "End date cannot be before start time"))
    .required('End Date is required')
    .typeError('Enter a value End date')
});


export default function DateValidationWithFormik() {
	return (
		<div>
			<h1>Formik Validate Date Pickers</h1>
			<LocalizationProvider dateAdapter={AdapterDateFns}>
				<Formik
					initialValues={{
						startDate: '',
						endDate: '',
					}}
					validationSchema={DisplayingErrorMessagesSchema}
					onSubmit={(values, { setSubmitting }) => {
						setTimeout(() => {
							setSubmitting(false);
							alert(JSON.stringify(values, null, 2));
						}, 500);
					}}
				>
					{({ errors, touched, values }) => (
					<Form>
						<Field 
							component={DesktopDatePicker}
							disablePast
							views={['year', 'month', 'day']}
							name="startDate"
							label="Start Date"              
						/>
						{touched.startDate && errors.startDate && <div>{errors.startDate}</div>}

						<Field              
							component={DesktopDatePicker}
							name="endDate"
							label="End Date"
							views={['year', 'month', 'day']}

						/>
						{touched.endDate && errors.endDate && <div>{errors.endDate}</div>}
				   
						<Button
							type="submit"
							variant="contained"
							color="primary"
						>
							Submit
						</Button>

					</Form>
					)}
				</Formik>
			</LocalizationProvider>
		</div>
	)  
}
```
