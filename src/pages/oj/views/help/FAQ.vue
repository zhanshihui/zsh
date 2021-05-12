<template>
  <div style="margin: auto; width:85%">
  <panel>
    <div slot="title">{{$t('m.Frequently_Asked_Questions')}}</div>
    <div class="content markdown-body">
      <ul>
        <li>{{$t('m.Where_is_the_input_and_the_output')}} 
          <p>{{$t('m.Where_is_the_input_and_the_output_answer_part_1')}} <code>stdin</code> ('{{$t('m.Standard_Input')}}') {{$t('m.Where_is_the_input_and_the_output_answer_part_3')}} <code>stdout</code>
            ('{{$t('m.Standard_Output')}}') {{$t('m.Where_is_the_input_and_the_output_answer_part_5')}} <code>scanf</code> {{$t('m.Where_is_the_input_and_the_output_answer_part_6')}} <code>cin</code>
            {{$t('m.Where_is_the_input_and_the_output_answer_part_7')}} <code>printf</code> {{$t('m.Where_is_the_input_and_the_output_answer_part_8')}} <code>cout</code> {{$t('m.Where_is_the_input_and_the_output_answer_part_9')}} <code>{{$t('m.Runtime_Error')}}</code>.
          </p>
        </li>
        <li>「原力值」是什么？
          <p>「 ZOJ 」的原力值系统。
		        您可以通过签到、做题来获取原力值，到达一定原力值可以升级，并获得等级称号。
          </p>
          <p>原力值计算说明:
            <p>1、签到每次获得 <code>1</code> 原力值。在连续签到的第 <code>3、7、15、30、60、90、120、240、365</code> 天可以分别额外获得 <code>1、2、4、7、12、20、33、54、88</code> 原力值（没错，就是斐波那契数列）；</p>
            <p>2、每道题目，<strong>第一次 AC </strong>时根据难度（入门、普及-、普及+、提高-、提高+），可分别获得 <code>3、5、10、15、20</code> 原力值。</p>
          </p>
          <p>
            等级称号如下：
            <p>0 - 99 原力值: <Tag color="#82A2F2" style="margin-right:-15px;">萌新</Tag></p>
            <p>100 - 199 原力值: <Tag color="#9E6EE7" style="margin-right:-15px;">蒟蒻</Tag></p>
            <p>200 - 499 原力值: <Tag color="#6c5ce7" style="margin-right:-15px;">键盘侠</Tag></p>
            <p>500 - 999 原力值: <Tag color="#1dd1a1" style="margin-right:-15px;">天才</Tag></p>
            <p>1000 - 4999 原力值: <Tag color="#DE24F7" style="margin-right:-15px;">大佬</Tag></p>
            <p>5000 - 9999 原力值: <Tag color="#222f3e" style="margin-right:-15px;">神犇</Tag></p>
            <p>10000 - ∞ 原力值: <Tag color="#ee5253" style="margin-right:-15px;">AKIOI</Tag></p>
            <p>分别是0 - 6级（萌新都是从 0 开始的嘛），每个等级都有自己的称号</p>
            <p>请小伙伴们保持诚信刷题的好习惯，如首次发现抄袭者会给一个棕色称号哦！多次发现会有封号风险！<Tag color="#B45D0C" style="margin-right:-15px;">抄人</Tag>	</p>
            <p>申请个性称号满足一下两种条件之一：1、在一周完成题目在30题以上并且诚信做题的同学；2、在排行榜在第一页的同学。请在讨论帖或个性签名备注申请。（注：获得称号一段时间后经评估不符合条件同学将重置！）</p>
          </p>
        </li>
        <li>{{$t('m.What_is_the_meaning_of_submission_execution_time')}} 
        <p>{{$t('m.What_is_the_meaning_of_submission_execution_time_answer')}} 
        </p>
        </li>
        <li>{{$t('m.How_Can_I_use_CPP_Int64')}} 
          <p>{{$t('m.How_Can_I_use_CPP_Int64_answer_part_1')}}<code>long long</code> {{$t('m.How_Can_I_use_CPP_Int64_answer_part_2')}} <code>cin/cout</code> {{$t('m.or')}} <code>%lld</code>, {{$t('m.using')}}<code> __int64</code> {{$t('m.How_Can_I_use_CPP_Int64_answer_part_3')}} <code>{{$t('m.Compile_Error')}}</code>.</p>
        </li>
        <li>{{$t('m.Java_specifications')}}
          <p>{{$t('m.Java_specifications_answer_part_1')}} <code>Main</code> {{$t('m.Java_specifications_answer_part_2')}} <code>Main</code> {{$t('m.Java_specifications_answer_part_3')}}</p>
        </li>
        <li>{{$t('m.About_presentation_error')}}
          <p>{{$t('m.About_presentation_error_answer_part_1')}} <b>{{$t('m.last')}}</b> {{$t('m.About_presentation_error_answer_part_2')}} <code> {{$t('m.Wrong_Answer')}}</code>.</p>
        </li>
        <li>{{$t('m.How_to_report_bugs')}}
          <p>{{$t('m.How_to_report_bugs_answer_part_1')}} <a href="https://github.com/Harry-zklcd">Github</a>
            {{$t('m.How_to_report_bugs_answer_part_2')}}
          </p>
        </li>
      </ul>
    </div>
  </panel>
  </div>
</template>

<script>
</script>

<style lang="less" scoped>
  .content {
    font-size: 16px;
    margin: 0 50px 40px 50px;
    > ul {
      list-style: disc;
      li {
        font-size: 16px;
        margin-top: 20px;
        &:first-child {
          margin-top: 0;
        }
        p {
          font-size: 14px;
          margin-top: 5px;
        }
      }
    }
  }
</style>