import React, { useState,useEffect } from 'react'

function InputandTable(props) {

       const [description,setdescription]=useState('');
       const [quantity,setquantity]=useState(0);
       const [rate,setrate]=useState(0);
       const [total,settotal]=useState(0);
       const [billamt,setbillamt]=useState(0)
       const [TrasportationChrg,setTrasportationChrg]=useState(0)
       const [totalamt,settotalamt]=useState(0)
       const [Advance,setAdvance]=useState(0)
       const [dueamount,setdueamount]=useState(0)


       const [order_cnt,setorder_cnt]=useState(1);


       function handlechange(e){
        var name =e.target.name;
        var value=e.target.value;
        switch(name){
            case 'ordr_description':setdescription(value) 
                break;
            case 'ordr_quantity':setquantity(parseFloat(value)) 
                break;
            case 'ordr_rate':setrate(parseFloat(value))
                break;
            case 'TrasportationChrg':
                                    if(Number.isNaN(value))
                                    {
                                        setTrasportationChrg(0)
                                    }
                                    else{
                                    setTrasportationChrg(value)
                                    }
                break;    
            case 'Advance': if(Number.isNaN(value))
                                    {
                                        setAdvance(0.0)
                                    }
                                    else{
                                    setAdvance(value)
                                    }
            break;    
            default:alert('unknown case')
        }
        console.log(e.target.name,e.target.value)
    }

    useEffect(() => {
        settotal(quantity*rate);
        }, [quantity,rate])

    function add_ordr(e){
    
    e.preventDefault()
    setorder_cnt(prevval => prevval+1)
    var payload={
        Order_Description:description,
        Order_Quantity:quantity,
        Order_Rate:rate,
        Order_Total:total
    }
    const  temp = [...props.sv,payload]
    props.ssv(temp)
    }

    function del_ordr(e,indx){
        setorder_cnt(prevval => prevval-1)
      var arr=[...props.sv]
      arr.splice(indx,1)
      props.ssv(arr)
    }

    useEffect(() => {
        var Total_Amt=0.0;
        props.sv.map((item)=>{
            Total_Amt= Total_Amt+item.Order_Total
        })
        // console.log(Total_Amt)
        setbillamt(Total_Amt)
    }, [order_cnt])

    useEffect(() => {
   
        var temp=Number(TrasportationChrg) + billamt
        settotalamt(temp)
        
    }, [billamt,TrasportationChrg])

    useEffect(() => {

       var temp = totalamt-Advance
       setdueamount(temp)
    }, [totalamt,Advance])
  
    return (
        <>
            <div>
                <button onClick={add_ordr}>add ordr +</button>
                <input name="ordr_description" value={description} onChange={handlechange} placeholder="desctiption" type="text"/>
                <input name="ordr_quantity" value={quantity} onChange={handlechange} placeholder="quantity" type="number"/>
                <input name="ordr_rate" value={rate} onChange={handlechange} placeholder="rate" type="number"/>
                <input name="ordr_total" value={total} placeholder="total" disabled type="number"/>
            </div>
            <table style={{"width":"100vw"}}>
                <tr>
                    <th>
                        [+/-]
                    </th>
                    <th>
                        Descrtiption
                    </th>
                    <th>
                        Quantity
                    </th>
                    <th>
                        Rate
                    </th>
                    <th>
                        Amount
                    </th>

                </tr>
                    {
                        props.sv.map((k,indx)=>{                  
                            return <tr key={indx}>
                                <td><button onClick={(e)=>{del_ordr(e,indx)}}>del-ordr</button></td>
                                <td>{k.Order_Description}</td>
                                <td>{k.Order_Quantity}</td>
                                <td>{k.Order_Rate}</td>
                                <td>{k.Order_Total}</td>
                            </tr>
                        })
                    }
            </table>
            <div>bill amount is{billamt}</div>
            <label htmlFor="idi">Transportation charge:</label>
            <input id="idi" name="TrasportationChrg" value={TrasportationChrg || 0} onChange={handlechange} placeholder="transport" type="number"/>
           <div>Total Amt is....{totalamt}</div>
            <div>
            <label htmlFor="ids">Advance charge:</label>
            <input id="ids" name="Advance" value={Advance || 0} onChange={handlechange} placeholder="advance" type="number"/>
            <div>Due amount is...{dueamount}</div>
            </div>
        
            {order_cnt}
         

            <div>{billamt            }</div>        
            <div>{TrasportationChrg  }</div>
            <div>{totalamt           }</div>
            <div>{Advance            }</div>
            <div>{dueamount          }</div>
        </>
    )
}

export default InputandTable



//.............................................................................................................................................

import React,{useState} from 'react'
import InputandTable from './InputandTable'
function OrderTable() {

    const [table,settable]=useState([])

    return (
        <>
     <InputandTable sv={table} ssv={settable}/>
     {/* {JSON.stringify(table)} */}
     </>
    )
}

export default OrderTable

//...............................................................................................................................................

import React from 'react';
import './App.css';

import MovieApp from './StateComponents/MovieApp'
import ReactRouterApp from './Routercomponents/ReactRouter'
import OrdrTable from './DynamicTable/OrderTable'

function App() {
  return (
    <div className="App">
    {/* <MovieApp/> */}
    {/* <ReactRouterApp/> */}
    <OrdrTable/>
    </div>
  );
}

export default App;

//...............................................................
 app.use(function (req, res, next) {
    //Enabling CORS
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Methods", "GET,HEAD,OPTIONS,POST,PUT");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, 
    Accept, x-client-key, x-client-token, x-client-secret, Authorization");
      next();
    });





