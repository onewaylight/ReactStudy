```js
      <Formik  
        validationSchema={UserValidationSchema}  
        initialValues={{  
          firstName: '',  
          lastName: '',  
          emailId: '',  
          password: '',  
          confirmPassword: '',  
          mobileNo: '',  
          address: '',  
          pinCode: '',  
          companyName: '',  
        }}  
    
        onSubmit={async (input, { resetForm }) => {  
          await Axios.post(`${apiUrl}/InsertUserData`, input)
          .then(res => {  
              toast.success(res.data)  
              this.bindUserData();  
              resetForm({})  
            }
          )  
          .catch(err => {  
            toast.error('Something went wrong.');  
          });  
        }}  
```
